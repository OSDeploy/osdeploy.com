---
description: >-
  Follow how Update-OSDeployCoreOS discovers and caches Microsoft inbox Ethernet
  and Wi-Fi drivers for WinPE.
---

# Insider: Export WinPE Drivers

This article follows the network-driver extraction path in `Update-OSDeployCoreOS`. While the exported Enterprise image is mounted read-only, OSDeploy identifies Microsoft inbox Ethernet and Wi-Fi packages and copies their DriverStore content into the WinPE driver repository.

This is a targeted export. It does not enumerate every installed driver and it does not collect OEM driver packs.

{% hint style="info" %}
The extracted files are Microsoft inbox network drivers represented by Windows servicing packages. Use the separate WinPE driver workflows for OEM and third-party driver sources.
{% endhint %}

## Follow the Code Path

Ethernet and Wi-Fi use the same processing pipeline:

1. Find matching servicing-package manifests.
2. Parse package identity and INF metadata.
3. Remove incomplete records.
4. Keep the highest package version for each driver name.
5. Locate the matching package in DriverStore.
6. Copy it into the architecture-specific OSDeploy repository.

The two passes differ only in their package-name pattern and destination family.

## Locate the Driver Sources

The read-only Windows mount supplies two source directories:

```
Windows\servicing\Packages\
Windows\System32\DriverStore\FileRepository\
```

The servicing directory contains `.mum` manifests that describe Windows packages. The DriverStore contains the files referenced by those packages.

OSDeploy searches for these manifest patterns:

| Driver family | Manifest pattern                          |
| ------------- | ----------------------------------------- |
| Ethernet      | `Microsoft-Windows-Ethernet-Client-*.mum` |
| Wi-Fi         | `Microsoft-Windows-Wifi-Client-*.mum`     |

If the servicing directory does not exist or no matching manifests are found, the function writes verbose diagnostics and continues.

## Parse the Package Manifest

Each `.mum` file is XML with a namespace. The function loads it with `System.Xml.Linq.XDocument`, reads the root namespace, and then resolves the package identity and first INF element through that namespace:

```powershell
$MumXml = [System.Xml.Linq.XDocument]::Load($mumPath)
$ns = $MumXml.Root.Name.Namespace
$Identity = $MumXml.Root.Element($ns + 'assemblyIdentity')
$DriverInf = $MumXml.Root.Descendants($ns + 'inf') |
	Select-Object -First 1 |
	ForEach-Object { $_.Value }
```

OSDeploy extracts four values:

| Value        | XML source                                           | Use                                     |
| ------------ | ---------------------------------------------------- | --------------------------------------- |
| Name         | `assemblyIdentity` `name` attribute                  | Logical driver name                     |
| Version      | `assemblyIdentity` `version` attribute               | Version comparison and destination path |
| Architecture | `assemblyIdentity` `processorArchitecture` attribute | Architecture repository                 |
| INF file     | First `inf` element                                  | DriverStore folder lookup               |

The package prefix and `-FOD-Package` suffix are removed from the logical name. For example, an Ethernet package name is normalized by:

```powershell
$identityName `
	-replace '^Microsoft-Windows-Ethernet-Client-', '' `
	-replace '-FOD-Package$', ''
```

## Skip Incomplete Metadata

A manifest becomes a driver record only when all required values are present. OSDeploy checks for:

* The `assemblyIdentity` element.
* The `name`, `version`, and `processorArchitecture` attributes.
* An INF reference.

Incomplete manifests are skipped rather than allowed to create invalid paths or version objects. The reason is shown only with `-Verbose`, allowing the import to continue when a Windows image contains an unexpected package manifest.

## Keep the Highest Version

Multiple manifests can describe the same normalized driver name. OSDeploy uses a hashtable keyed by name and replaces the stored record only when it finds a higher version:

```powershell
$dedupHash = @{}

foreach ($driver in $EthernetDrivers) {
	if (
		-not $dedupHash.ContainsKey($driver.Name) -or
		$driver.Version -gt $dedupHash[$driver.Name].Version
	) {
		$dedupHash[$driver.Name] = $driver
	}
}

$EthernetDrivers = $dedupHash.Values
```

Wi-Fi uses the same logic in a separate hashtable. Deduplication occurs within the current image and driver family.

## Resolve the DriverStore Package

The INF file name connects servicing metadata to DriverStore. OSDeploy removes the `.inf` extension and searches for the first directory whose name begins with that INF base:

```powershell
$InfBase = [System.IO.Path]::GetFileNameWithoutExtension($Driver.InfFile)
$DriverFolder = [System.IO.Directory]::EnumerateDirectories(
	$driverStoreRepo,
	"$InfBase*"
) | Select-Object -First 1
```

A typical DriverStore directory appends architecture and content identifiers to the INF base, so an exact directory-name comparison would not work.

If no matching directory is found, the function writes a verbose message and moves to the next record. It does not create an empty destination.

## Build the Repository Path

Driver packages are stored under:

```
C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\
```

The complete path is organized by architecture, family and version, and normalized driver name:

```
winpe-drivers\
└── amd64\
	├── microsoft-windows-ethernet-<version>\
	│   └── <driver-name>\
	└── microsoft-windows-wifi-<version>\
		└── <driver-name>\
```

ARM64 packages use the parallel `arm64` directory when the servicing metadata reports that architecture.

| Path segment                                                 | Source                                            |
| ------------------------------------------------------------ | ------------------------------------------------- |
| `amd64` or `arm64`                                           | `processorArchitecture` from the package identity |
| `microsoft-windows-ethernet-*` or `microsoft-windows-wifi-*` | Driver family and package version                 |
| Final directory                                              | Normalized package name                           |

## Copy or Reuse the Package

Before copying, the function checks whether the destination already contains files:

```powershell
if (Test-Path "$EthernetDst\*") {
	Write-Verbose "Skipping existing Ethernet driver"
}
else {
	robocopy $DriverFolder $EthernetDst *.* /e /r:0 /w:0
}
```

Existing populated destinations are reused. New packages are copied recursively with no retry delay and no automatic retry. Robocopy output is suppressed from the PowerShell pipeline and appended to the current OS-file log.

The public function still returns the imported Windows OS directory, not a list of driver directories.

## Understand the Architecture Boundary

The command can process AMD64 and ARM64 ESDs in one run. Driver discovery occurs separately while each exported Enterprise image is mounted.

The architecture in the destination path comes from the package manifest rather than from the public `-Architecture` parameter. In normal media these values align, but using the manifest keeps each package tied to its own servicing identity.

The repository path also includes the package version. A newer Windows ESD can therefore add a newer family directory without overwriting the older package set. The separate `Update-OSDeployCoreDrivers` workflow can later manage the broader WinPE driver repository.

## Review Driver Diagnostics

Use verbose output to inspect manifest counts, parsed identities, deduplicated record counts, DriverStore searches, copy destinations, and skip reasons:

```powershell
Update-OSDeployCoreOS -Architecture amd64 -Verbose
```

Inspect the cached network-driver families:

```powershell
$DriverRoot = 'C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers'

Get-ChildItem -Path $DriverRoot -Directory -Recurse |
	Where-Object Name -Like 'microsoft-windows-*' |
	Select-Object FullName
```

{% hint style="warning" %}
A missing driver directory does not necessarily mean the OS import failed. Manifests with incomplete metadata and INF references that cannot be resolved to DriverStore are skipped with verbose output.
{% endhint %}

## Related

* [Update Windows 11 OS](../cmdlets/update-osdeploycoreos.md)
* [Insider: Building an OS from an ESD](insider-export-windows-11.md)
* [Insider: Exporting Windows RE](insider-export-windows-re.md)
* [Update WinPE Drivers](../cmdlets/update-osdeploycoredrivers.md)
* [Update-OSDeployCoreOS command reference](../../command-reference/osdeploy/update-osdeploycoreos.md)

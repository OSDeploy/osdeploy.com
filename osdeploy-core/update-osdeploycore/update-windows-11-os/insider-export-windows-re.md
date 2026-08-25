---
description: >-
  Follow how Update-OSDeployCoreOS extracts Windows RE and creates a separate
  recovery-image cache.
---

# Insider: Export Windows RE

This article follows the Windows RE path inside `Update-OSDeployCoreOS`. The function extracts the recovery image from the Enterprise operating system and creates a separate cache that preserves its relationship to the source Windows build.

Windows RE is not taken directly from a fixed ESD index. It is collected from the installed operating-system image after Enterprise has been exported to `install.wim`.

## Follow the Code Path

Windows RE processing happens in two phases:

1. Mount `install.wim` read-only and extract the recovery files and image metadata.
2. Build a parallel `windows-re` directory from selected content in the Windows OS import.

This keeps one copy of the full setup media under `windows-os` and a smaller recovery-specific source under `windows-re`.

## Locate Windows RE

The function mounts the exported Enterprise image and looks in the standard recovery directory:

```
Windows\System32\Recovery\
```

It tests for two files independently:

| Source file   | OSDeploy destination   | Purpose                                   |
| ------------- | ---------------------- | ----------------------------------------- |
| `ReAgent.xml` | `.temp\os-reagent.xml` | Recovery configuration from the source OS |
| `winre.wim`   | `.wim\winre.wim`       | Windows Recovery Environment image        |

The relevant code does not assume either file exists:

```powershell
$winreSource = Join-Path $MountDirectory 'Windows\System32\Recovery\winre.wim'
$reagentSource = Join-Path $MountDirectory 'Windows\System32\Recovery\ReAgent.xml'

if (Test-Path $reagentSource) {
	Copy-Item -Path $reagentSource -Destination (Join-Path $DestinationTemp 'os-reagent.xml')
}

if (Test-Path $winreSource) {
	Copy-Item -Path $winreSource -Destination (Join-Path $DestinationWim 'winre.wim')
}
```

If `ReAgent.xml` is missing, the recovery image can still be copied. If `winre.wim` is missing, no WinRE image metadata or recovery `properties.json` is created.

## Inspect the Recovery Image

After copying `winre.wim`, the function opens index 1 with `Get-WindowsImage` and writes three metadata files under the Windows OS `.core` directory:

| File                            | Content                                       |
| ------------------------------- | --------------------------------------------- |
| `winre-windowsimage.json`       | JSON representation of the image properties   |
| `winre-windowsimage.xml`        | CLIXML representation of the image properties |
| `winre-windowsimagecontent.txt` | File listing from `Get-WindowsImageContent`   |

These files describe the recovery WIM before the separate recovery directory is built. They travel with the copied `.core` content into that directory.

## Build the Recovery Cache

The Windows OS import and Windows RE import use the same destination ID:

```
26200.8653-amd64-enterprise-en-us
```

Their roots are different:

```
C:\ProgramData\OSDeployCore\cache\windows-os\26200.8653-amd64-enterprise-en-us\
C:\ProgramData\OSDeployCore\cache\windows-re\26200.8653-amd64-enterprise-en-us\
```

The function creates the recovery cache with three robocopy operations:

| Windows OS source | Windows RE destination | Included content                                            |
| ----------------- | ---------------------- | ----------------------------------------------------------- |
| `.core\`          | `.core\`               | Core files except OS image data and WinPE/WinSetup metadata |
| `.temp\`          | `.temp\`               | Extracted temporary content except logs                     |
| `.wim\`           | `.wim\`                | `winre.wim` only                                            |

The exclusions are deliberate:

```powershell
robocopy $OSCore $RECore *.* /e `
	/xf OSImage.* winpe-windowsimage* winse-windowsimage*

robocopy $OSTemp $RETemp *.* /e /xd logs
robocopy $OSWim $REWim winre.wim /e
```

The recovery directory does not need the setup `boot.wim`, Enterprise `install.wim`, separate WinPE or WinSetup WIMs, or their metadata. It retains the recovery image and supporting OS-derived content used by later build steps.

{% hint style="info" %}
The `windows-re` cache is derived from the matching `windows-os` import. The shared destination ID makes that source relationship explicit across architecture and Windows build updates.
{% endhint %}

## Write WinRE Properties

The function reads the copied recovery WIM from its final location and creates `properties.json` in the root of the `windows-re` directory.

The file combines two kinds of data:

| Property group          | Examples                                                         | Source                            |
| ----------------------- | ---------------------------------------------------------------- | --------------------------------- |
| Recovery image identity | `Version`, `Architecture`, `Languages`, `ImageSize`, `FileCount` | `winre.wim` index 1               |
| Source OS identity      | `OSImageName`, `OSEditionId`, `OSVersion`, `OSCreatedTime`       | Exported Enterprise `install.wim` |
| Cache location          | `Path`, `ImagePath`, `ImageIndex`                                | Final `windows-re` directory      |

This allows a consumer to inspect the recovery image and confirm which Windows OS image supplied it without reopening both WIM files.

An abbreviated properties object looks like this:

```json
{
  "Type": "WinRE",
  "Id": "26200.8653-amd64-enterprise-en-us",
  "Architecture": "amd64",
  "Version": "10.0.26200.8653",
  "OSImageName": "Windows 11 Enterprise",
  "OSEditionId": "Enterprise",
  "ImageIndex": 1
}
```

Exact versions and image properties depend on the ESD imported by the installed OSDeploy module.

## Understand Missing Recovery Content

The function tests for `winre.wim` before copying it and again before writing recovery properties. A missing image therefore does not produce a terminating error in this section.

The Windows OS import can finish without a `windows-re\properties.json`, but that result is incomplete for workflows that require a recovery source. Use `-Verbose` and inspect the source image when expected recovery files are absent.

The mount itself remains protected by the outer `try` and `finally` block. Whether recovery extraction succeeds or not, OSDeploy dismounts the Windows image with `-Discard` and removes the temporary mount directory when possible.

## Inspect an Imported Recovery Image

Read the recovery properties:

```powershell
$WindowsRERoot = 'C:\ProgramData\OSDeployCore\cache\windows-re'
$ImportedRE = Get-ChildItem -Path $WindowsRERoot -Directory |
	Sort-Object Name -Descending |
	Select-Object -First 1

Get-Content -Path (Join-Path $ImportedRE.FullName 'properties.json') -Raw |
	ConvertFrom-Json |
	Select-Object Id, Architecture, Version, OSImageName, OSVersion
```

Inspect the recovery WIM directly:

```powershell
$WinREWim = Join-Path $ImportedRE.FullName '.wim\winre.wim'
Get-WindowsImage -ImagePath $WinREWim -Index 1
```

## Related

* [Update Windows 11 OS](./)
* [Insider: Building an OS from an ESD](insider-export-windows-11.md)
* [Insider: Exporting WinPE Drivers from an OS](export-winpe-drivers.md)
* [Update-OSDeployCoreOS command reference](../../../powershell-modules/osdeploy/Update-OSDeployCoreOS.md)
* [Build-OSDeployBoot command reference](../../../powershell-modules/osdeploy/Build-OSDeployBoot.md)

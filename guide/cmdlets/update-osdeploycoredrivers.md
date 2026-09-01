---
description: Download and prepare WinPE driver packages for OSDeploy Core boot images.
---

# Update-OSDeployCoreDrivers

{% hint style="info" %}
`Update-OSDeployCoreDrivers` is the third public OSDeploy PowerShell module sub-function run by `Update-OSDeployCore`. Run it independently when only the WinPE driver library needs updating.

**TLDR:** Run `Update-OSDeployCoreDrivers` to update all active driver sources. Use `-Name` only when specific sources are needed.

```powershell
Update-OSDeployCoreDrivers
Update-OSDeployCoreDrivers -Name 'dell', 'hp'
Update-OSDeployCoreDrivers -Name 'intel-ethernet' -DownloadOnly
```
{% endhint %}

`Update-OSDeployCoreDrivers` prepares the network and storage drivers that `Build-OSDeployBoot` can inject into boot images. It discovers the latest packages from each requested source, downloads the archives, and expands them into versioned folders in the OSDeploy Core library.

All matching packages are processed automatically. The command does not display a package picker or ask for confirmation unless `-Confirm` is specified.

<figure><img src="../../.gitbook/assets/image (424).png" alt=""><figcaption></figcaption></figure>

## Requirements

Run this command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`. Dynamic sources require access to their vendor metadata endpoints; uncached packages require access to their download URLs.

{% hint style="warning" %}
The command stops before driver processing when a Windows version, PowerShell, MSI installation, `curl.exe`, or administrator check fails. A failure for one catalog source or package is otherwise reported and processing continues with the remaining entries.
{% endhint %}

## Parameters

| Parameter          | Type             | Default     | Accepted values and behavior                                                                                                                                                                                                                                    |
| ------------------ | ---------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-Name`            | `String[]`       | Automatic   | Positional parameter 0. Omission refreshes every configured source with an update or download URI, then processes every enabled resolved package. Tab completion suggests enabled refreshable sources. Unknown or nonrefreshable values are warned and skipped. |
| `-Force`           | `Switch`         | Not enabled | Re-downloads matching package archives instead of reusing a valid cache file. The catalog helper accepts this switch but does not change discovery behavior. Existing nonempty Dell, HP, and Intel expanded folders are still reused.                           |
| `-SkipWifiDrivers` | `Switch`         | Not enabled | Excludes package names containing `wifi` or `wireless`. The command enables this automatically when no valid imported WinRE source is found.                                                                                                                    |
| `-DownloadOnly`    | `Switch`         | Not enabled | Returns after saving package archives in the download cache. Expansion and `package.json` creation are skipped.                                                                                                                                                 |
| `-Architecture`    | `String`         | Automatic   | Use `amd64` or `arm64`. Omission permits both. The filter applies to package processing, not catalog refresh, and packages with a blank architecture remain eligible.                                                                                           |
| `-WhatIf`          | Common parameter | Not enabled | Runs catalog and package discovery with child preview enabled. Protected catalog writes, downloads, and expansion are skipped, but initialization, vendor requests, catalog-directory creation, and cache reads can still occur.                                |
| `-Confirm`         | Common parameter | Not enabled | Prompts once before the combined catalog refresh and package-processing operation. Child confirmation prompts are suppressed.                                                                                                                                   |

## Driver Sources

The active source names are defined by the installed OSDeploy module. The current sources are:

| Name             | Package                                     | Architecture |
| ---------------- | ------------------------------------------- | ------------ |
| `dell`           | Dell WinPE 11 driver pack                   | amd64        |
| `hp`             | HP WinPE driver pack                        | amd64        |
| `intel-ethernet` | Intel Ethernet adapter driver pack          | amd64        |
| `intel-wifi`     | Intel wireless IT administrator driver pack | amd64        |

Use tab completion after `-Name` to view the active sources in the installed module:

```powershell
Update-OSDeployCoreDrivers -Name <Tab>
```

The installed module also contains disabled source definitions that are not offered by completion and are not returned for package processing. Source availability can change with the installed OSDeploy version; the module configuration, not this table, controls the effective set.

## Catalog Selection and Merge

Dynamic sources use vendor-specific discovery. A source with only a download URI uses its static module metadata. The refreshed records are merged into `%ProgramData%\OSDeployCore\cache\config\winpedrivers.json` with the current catalog date.

When `-Name` limits the refresh, entries for other sources are preserved. A requested source that returns no data or fails discovery also keeps its existing catalog entry. Catalog output is suppressed by `Update-OSDeployCoreDrivers`; only package files are returned.

## Download All Drivers

Run the command without `-Name` to refresh every active source and process every matching package:

```powershell
Update-OSDeployCoreDrivers
```

The command:

1. Discovers the latest package metadata from each vendor.
2. Updates the local WinPE driver catalog.
3. Downloads each matching package that is not already cached.
4. Verifies a package checksum when the vendor catalog provides one.
5. Expands the package into the OSDeploy Core driver library when its destination does not already contain files.
6. Writes `package.json` metadata beside the expanded driver files when that metadata file does not already exist.

If one source cannot be discovered or processed, the command writes a warning and continues with the remaining sources.

## Download Selected Sources

Use `-Name` to limit catalog refresh and package processing to one or more sources:

```powershell
Update-OSDeployCoreDrivers -Name 'dell'
```

```powershell
Update-OSDeployCoreDrivers -Name 'dell', 'hp', 'intel-ethernet'
```

Refreshing selected sources preserves the other entries already stored in the local catalog. Passing a disabled source name directly can refresh its catalog metadata when it has a URI, but disabled sources are skipped during package resolution.

## Select an Architecture

Use `-Architecture` to process packages matching `amd64` or `arm64`:

```powershell
Update-OSDeployCoreDrivers -Architecture amd64
```

The current active vendor packages are amd64. An ARM64 filter can return no matching packages unless the installed OSDeploy configuration contains an active ARM64 source.

## Wi-Fi Drivers

ADK WinPE does not support wireless hardware. Wi-Fi drivers are used only when building from an imported Windows RE source.

Exclude wireless packages explicitly:

```powershell
Update-OSDeployCoreDrivers -SkipWifiDrivers
```

When no imported OS sources are available, the command enables `-SkipWifiDrivers` automatically and writes a warning. Import Windows into OSDeploy Core before downloading Wi-Fi drivers for a WinRE-based boot image.

{% hint style="warning" %}
Do not expect Intel Wi-Fi drivers to add wireless support to an ADK WinPE boot image. Include them only for a WinRE-based image.
{% endhint %}

## Download Without Expanding

Use `-DownloadOnly` to save package archives without expanding them:

```powershell
Update-OSDeployCoreDrivers -Name 'dell' -DownloadOnly
```

Raw downloads are stored below a vendor-specific folder:

```
C:\ProgramData\OSDeployCore\cache\downloads\<vendor>\
```

In download-only mode, the command does not create the expanded driver folder or write `package.json` metadata.

## Driver Library

During normal processing, expanded drivers and package metadata are stored in the architecture-specific module-managed cache:

```
C:\ProgramData\OSDeployCore\cache\winpedrivers-<architecture>\<name>-<version>\
```

List the module-managed driver folders after processing:

```powershell
Get-ChildItem -Path "$env:ProgramData\OSDeployCore\cache\winpedrivers-*\*" -Directory |
	Select-Object Name, Parent, LastWriteTime, FullName
```

User-managed drivers remain separate under `%ProgramData%\OSDeployCore\repository\winpedrivers-amd64` and `%ProgramData%\OSDeployCore\repository\winpedrivers-arm64`. `Update-OSDeployCoreDrivers` does not add, update, or remove content in those repository folders.

## Refresh Cached Packages

Existing downloads and expanded driver folders are reused when possible. A package with a published SHA256 is reused only when the cache matches; a package without a checksum is reused when the expected file exists. The downloader can also copy a differently named file from the same vendor cache when its SHA256 matches the requested package.

Add `-Force` to re-download matching packages:

```powershell
Update-OSDeployCoreDrivers -Name 'dell' -Force
```

`-Force` does not force catalog discovery to return different metadata, and it does not clear a populated Dell, HP, or Intel expansion directory. Remove an obsolete expanded package directory separately when a clean re-expansion is required.

## Preview the Operation

Use `-WhatIf` to show the catalog and package actions without downloading or expanding drivers:

```powershell
Update-OSDeployCoreDrivers -Name 'dell', 'hp' -WhatIf
```

The public command takes a dedicated preview path so child commands can report their planned actions. It can still initialize and migrate Core paths, inspect imported WinRE sources, contact vendors, load the existing catalog, and create the catalog parent directory. The catalog file is not written and package handlers are not invoked because child `ShouldProcess` calls decline the mutations.

Use `-Verbose` to display source discovery, catalog merge, package selection, cache, and expansion details:

```powershell
Update-OSDeployCoreDrivers -Name 'dell' -Verbose
```

## Manual Download Fallback

Some vendors can require a browser agreement or block automated requests with a web application firewall. When this happens, the command skips that package, continues processing the remaining sources, and prints the URL, expected file name, and destination for a manual download.

Place the downloaded file in the displayed cache path, then rerun the command for that source:

```powershell
Update-OSDeployCoreDrivers -Name 'dell'
```

## Review the Results

The command returns a `System.IO.FileInfo` object for each package file processed successfully. This can be a newly downloaded file, a reused valid cache file, or a file copied from an alternate checksum-matching cache entry. `-DownloadOnly` returns the same file type. No object is returned for catalog files, unmatched packages, disabled sources, declined operations, or failed packages.

```powershell
$DriverPackages = Update-OSDeployCoreDrivers -Name 'dell', 'hp'
$DriverPackages | Select-Object Name, Length, DirectoryName
```

Expanded driver folders are consumed later by `Build-OSDeployBoot` when it creates a boot image.

## Related

* [Complete OSDeploy Core update](update-osdeploycore.md)
* [Update-OSDeployCoreDrivers command reference](../../command-reference/osdeploy/update-osdeploycoredrivers.md)
* [WinPE Drivers overview](/broken/pages/CVKBMSZoucH2as3D7VFG)
* [Dell WinPE Drivers](/broken/pages/XTULK4EcB6qhwoPfk2nw)
* [HP WinPE Drivers](/broken/pages/LJaCHx9QaXP2COtZ7kwA)
* [Intel Ethernet Drivers](/broken/pages/b3NP3NTDXSeoxygsSeDN)
* [Intel Wireless Drivers](/broken/pages/xR4uES51OgrLBs0FmKKO)
* [System Requirements](../requirements/windows-11-os.md)

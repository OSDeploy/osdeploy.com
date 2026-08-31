---
description: Download and prepare WinPE driver packages for OSDeploy Core boot images.
---

# Core WinPE Drivers

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

<figure><img src="../../../.gitbook/assets/image (424).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Run this command from an elevated PowerShell 7 session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.
{% endhint %}

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
5. Expands the package into the OSDeploy Core driver library.
6. Writes `package.json` metadata beside the expanded driver files.

If one source cannot be discovered or processed, the command writes a warning and continues with the remaining sources.

## Download Selected Sources

Use `-Name` to limit catalog refresh and package processing to one or more sources:

```powershell
Update-OSDeployCoreDrivers -Name 'dell'
```

```powershell
Update-OSDeployCoreDrivers -Name 'dell', 'hp', 'intel-ethernet'
```

Refreshing selected sources preserves the other entries already stored in the local catalog.

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

During normal processing, expanded drivers and package metadata are stored in versioned folders:

```
C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\<architecture>\<name>-<version>\
```

List the available driver folders after processing:

```powershell
Get-OSDeployCoreDrivers
```

Limit the results to amd64 and exclude Wi-Fi folders:

```powershell
Get-OSDeployCoreDrivers -Architecture amd64 -SkipWifiDrivers
```

## Refresh Cached Packages

Existing downloads and expanded driver folders are reused when possible. Add `-Force` to refresh the selected catalog entries and re-download matching packages:

```powershell
Update-OSDeployCoreDrivers -Name 'dell' -Force
```

## Preview the Operation

Use `-WhatIf` to show the catalog and package actions without downloading or expanding drivers:

```powershell
Update-OSDeployCoreDrivers -Name 'dell', 'hp' -WhatIf
```

The command can still contact vendor sources to discover current package metadata while processing `-WhatIf`.

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

The command returns a `System.IO.FileInfo` object for each package file processed successfully.

```powershell
$DriverPackages = Update-OSDeployCoreDrivers -Name 'dell', 'hp'
$DriverPackages | Select-Object Name, Length, DirectoryName
```

Expanded driver folders are consumed later by `Build-OSDeployBoot` when it creates a boot image.

## Related

* [Complete OSDeploy Core update](../osdeploycore/update-osdeploycore.md)
* [Update-OSDeployCoreDrivers command reference](../../../command-reference/osdeploy/update-osdeploycoredrivers.md)
* [WinPE Drivers overview](../../../core-components/winpe-drivers/)
* [Dell WinPE Drivers](../../../core-components/winpe-drivers/dell.md)
* [HP WinPE Drivers](../../../core-components/winpe-drivers/hp.md)
* [Intel Ethernet Drivers](../../../core-components/winpe-drivers/intel-ethernet.md)
* [Intel Wireless Drivers](../../../core-components/winpe-drivers/intel-wireless.md)
* [System Requirements](../../requirements/windows-11-os.md)

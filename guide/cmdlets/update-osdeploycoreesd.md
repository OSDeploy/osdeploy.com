---
description: Download and verify Windows 11 25H2 Enterprise ESD files for OSDeploy Core.
---

# Update-OSDeployCoreESD

{% hint style="info" %}
`Update-OSDeployCoreESD` is the first public OSDeploy PowerShell module sub-function run by `Update-OSDeployCore`. Run it independently when only the Windows ESD source files need updating.

**TLDR:** Run `Update-OSDeployCoreESD` to download the latest Windows 11 25H2 Enterprise ESD files in the OSDeploy catalog. Use `-Architecture` only when one architecture is needed.

```powershell
Update-OSDeployCoreESD
Update-OSDeployCoreESD -Architecture amd64
Update-OSDeployCoreESD -Architecture arm64
```
{% endhint %}

`Update-OSDeployCoreESD` downloads the current en-US Windows 11 25H2 Enterprise ESD files from the Microsoft Content Delivery Network. Download URLs, file names, sizes, and SHA256 checksums come from the operating system catalog included with the OSDeploy module.

This command downloads and caches the source ESD files. It does not install Windows or import the ESD contents into OSDeploy Core.

<figure><img src="../../.gitbook/assets/image (353).png" alt=""><figcaption></figcaption></figure>

## Requirements

Run this command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`. Internet access is required for URL tests and uncached downloads.

{% hint style="warning" %}
The command stops before catalog selection when a Windows version, PowerShell, MSI installation, `curl.exe`, or administrator check fails. It also stops when no catalog XML exists or the newest catalog file name does not match the expected `<build>-win<version>-<release>.xml` format.
{% endhint %}

## Parameters

| Parameter       | Type             | Default     | Accepted values and behavior                                                                                                                                                                                                             |
| --------------- | ---------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-Force`        | `Switch`         | Not enabled | Bypasses current and older verified cache reuse and queues a fresh download. It does not bypass the Yes/No download prompt.                                                                                                              |
| `-Architecture` | `String`         | Automatic   | Use `amd64` or `arm64`. Omission uses host-based targets: AMD64 hosts consider x64 then ARM64; ARM64 hosts consider ARM64 only. `amd64` on an ARM64 host leaves no eligible target.                                                      |
| `-WhatIf`       | Common parameter | Not enabled | Suppresses operations gated by `ShouldProcess`, including cache-directory creation, deletion, download, retry, and failed-file cleanup. Initialization, catalog and cache reads, hashing, URL tests, and Yes/No prompts can still occur. |
| `-Confirm`      | Common parameter | Not enabled | Adds PowerShell confirmation at each `ShouldProcess` boundary. Independent `ShouldContinue` Yes/No prompts still appear.                                                                                                                 |

## Select an Architecture

The architectures offered by default depend on the OSDeploy PC architecture.

| OSDeploy PC | Default ESD selection |
| ----------- | --------------------- |
| AMD64       | x64 first, then ARM64 |
| ARM64       | ARM64 only            |

Download only the x64 ESD on an AMD64 OSDeploy PC:

```powershell
Update-OSDeployCoreESD -Architecture amd64
```

Download only the ARM64 ESD:

```powershell
Update-OSDeployCoreESD -Architecture arm64
```

When `-Architecture` is omitted, the command processes every architecture supported by the current OSDeploy PC. Each ESD is confirmed separately before downloading.

## Download the ESD Files

Run the command without parameters to use the default architecture selection:

```powershell
Update-OSDeployCoreESD
```

For each ESD that is not already cached, the command:

1. Reads the latest operating system catalog included with OSDeploy.
2. Selects the en-US Enterprise ESD for the requested architecture.
3. Tests the Microsoft download URL for availability.
4. Displays the file name, size, and destination.
5. Prompts for confirmation.
6. Downloads the file with resumable transfer support.
7. Verifies the downloaded file against the catalog SHA256 checksum.

Downloads are saved in the version-specific folder:

```
C:\ProgramData\OSDeployCore\OSDCloud\OS\Windows 11 25H2\
```

The exact release folder is derived from the newest catalog file name, and the ESD file name can change when the module catalog is updated to a newer Windows build. Core path initialization occurs before catalog processing and can create directories or migrate legacy repository content and profiles.

{% hint style="warning" %}
Each ESD is several gigabytes. The confirmation prompt estimates a download time of 5 to 30 minutes, but the actual time depends on the internet connection.
{% endhint %}

## Use Cached Downloads

The command verifies an existing ESD before deciding whether another download is needed.

* If the current ESD exists and its SHA256 checksum matches, the file is returned without being downloaded again.
* If a differently named ESD from an older catalog exists in the current release folder and matches its own catalog checksum, the command offers a choice between returning that file and downloading the newer version. Older catalogs are cache lookup metadata, not fallback download sources.
* If an existing file does not match the expected checksum, the command offers to move it to the Recycle Bin and download it again.

Use `-Force` to download the selected ESD files again even when the current files are already cached and verified:

```powershell
Update-OSDeployCoreESD -Architecture amd64 -Force
```

## Preview the Operation

Use `-WhatIf` to prevent directory creation and downloads:

```powershell
Update-OSDeployCoreESD -Architecture amd64 -WhatIf
```

The command can still initialize Core paths, inspect and hash cached files, test download URLs, and display `ShouldContinue` prompts while processing `-WhatIf`. A verified current cache file, or an older verified file retained after declining the newer download, can still be returned. No ESD transfer starts unless both the Yes/No prompt and `ShouldProcess` approve it.

## Download Recovery

Downloads use `curl.exe` with resume support. When a transfer fails, produces no file, is incomplete, or fails SHA256 verification, the command makes up to two automatic retries after the initial attempt. URL reachability is tested for every pending entry before any per-file download confirmations are collected.

If all automatic attempts fail, the command displays the failure details and asks whether to retry again. Failed checksum files are moved to the Recycle Bin before a clean retry. If the final retry is declined, the failed file can also be moved to the Recycle Bin.

Use `-Verbose` to show catalog selection, cache checks, URL tests, architecture filtering, and download details:

```powershell
Update-OSDeployCoreESD -Architecture amd64 -Verbose
```

## Review the Results

The command returns one `System.IO.FileInfo` object for each ESD that is ready to use, whether it was downloaded during the current run or was already cached and verified.

```powershell
$WindowsESD = Update-OSDeployCoreESD -Architecture amd64
$WindowsESD | Select-Object Name, Length, DirectoryName
```

No object is returned for an architecture when its download is unavailable, declined, or unsuccessful.

## Import Windows into OSDeploy Core

After the ESD files are downloaded, run `Update-OSDeployCoreOS` to create the Windows media, WIM files, Windows RE content, and driver sources used by OSDeploy Core:

```powershell
Update-OSDeployCoreOS -Architecture amd64
```

## Related

* [Complete OSDeploy Core update](update-osdeploycore.md)
* [Update-OSDeployCoreESD command reference](../../command-reference/osdeploy/update-osdeploycoreesd.md)
* [Update-OSDeployCoreOS command reference](../../command-reference/osdeploy/update-osdeploycoreos.md)
* [Insider: The Windows ESD Catalog](../../osdeploy-insider/process/windows-esd-catalogs.md)
* [System Requirements](../requirements/windows-11-os.md)

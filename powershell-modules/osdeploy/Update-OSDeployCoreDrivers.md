# Update-OSDeployCoreDrivers

Downloads and expands WinPE driver packages into the OSDeployCore library.

| Property | Value                                                                 |
|----------|-----------------------------------------------------------------------|
| Module   | OSDeploy                                                              |
| Platform | Windows 11 (amd64 / arm64)                                           |
| Requires | PowerShell 7.6, internet access                                       |
| Output   | `System.IO.FileInfo`                                                  |

## Description

Refreshes the requested catalog sources, resolves the matching WinPE driver packages, and downloads and expands the selected packages to `$env:ProgramData\OSDeployCore`.

All matching packages are processed automatically without prompting. Use `-Name` to limit the refresh to one or more specific sources, `-SkipWifiDrivers` to exclude wireless packages, and `-DownloadOnly` to cache files without expanding them.

Wi-Fi driver sources are excluded automatically when no imported OS sources are present — ADK WinPE does not support wireless hardware, so Wi-Fi drivers are only needed when a WinRE-based boot image will be built.

After downloading, drivers are injected into WinPE at build time by `Build-OSDeployBoot`.

## Syntax

```powershell
Update-OSDeployCoreDrivers [[-Name] <String[]>] [-Force] [-SkipWifiDrivers] [-DownloadOnly] [[-Architecture] <String>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter          | Type       | Required | Description                                                                                                              |
|--------------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| `-Name`            | `String[]` | No       | One or more source names to refresh and process. When omitted, all configured sources are refreshed.                    |
| `-Force`           | `Switch`   | No       | Re-downloads driver packages even when a matching cached file already exists.                                            |
| `-SkipWifiDrivers` | `Switch`   | No       | Excludes Wi-Fi driver packages (`intel-wifi`, `microsoft-windows-wifi`). Enforced automatically when no imported OS sources exist. |
| `-DownloadOnly`    | `Switch`   | No       | Downloads packages to the cache without expanding them. No `package.json` metadata is written.                          |
| `-Architecture`    | `String`   | No       | Limits driver downloads to a single architecture: `amd64` or `arm64`. When omitted, all architectures are processed.   |

## Examples

```powershell
# Refresh the Dell catalog and download all matching packages
Update-OSDeployCoreDrivers -Name 'dell'
```

```powershell
# Refresh Dell and HP catalogs, preview without executing
Update-OSDeployCoreDrivers -Name 'dell', 'hp' -WhatIf
```

```powershell
# Refresh all configured sources and download all available packages
Update-OSDeployCoreDrivers
```

```powershell
# Refresh all configured sources, excluding Wi-Fi packages
Update-OSDeployCoreDrivers -SkipWifiDrivers
```

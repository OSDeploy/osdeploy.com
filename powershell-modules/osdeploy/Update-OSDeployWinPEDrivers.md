# Update-OSDeployWinPEDrivers

Downloads and expands WinPE driver packages into the OSDeployCore library.

| Property | Value                                                                 |
|----------|-----------------------------------------------------------------------|
| Module   | OSDeploy                                                              |
| Platform | Windows 11 (amd64 / arm64)                                           |
| Requires | PowerShell 7.6, internet access                                       |
| Output   | `System.IO.FileInfo`                                                  |

## Description

Refreshes one or more WinPE driver catalog sources, resolves the matching driver packages, and downloads and expands them to `$env:ProgramData\OSDeployCore`.

By default an Out-GridView picker is shown so you can choose which packages to download. Use `-NonInteractive` to skip the picker and process all matching packages without prompting.

Supported driver sources: `dell`, `hp`, `intel-ethernet`, `intel-wifi`, `microsoft-windows-ethernet-25h2`, `microsoft-windows-wifi-25h2`.

Wi-Fi driver sources are excluded automatically when no imported OS sources are present (ADK WinPE does not support wireless hardware).

After downloading, drivers are injected into WinPE at build time by `Build-OSDeployBootMedia`.

## Syntax

```powershell
Update-OSDeployWinPEDrivers [[-Name] <String[]>] [-Force] [-NonInteractive]
    [-SkipWifiDrivers] [-DownloadOnly] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter          | Type       | Required | Description                                                                                                                        |
|--------------------|------------|----------|------------------------------------------------------------------------------------------------------------------------------------|
| `-Name`            | `String[]` | No       | One or more driver source names to refresh. Tab completion is supported. When omitted, all configured sources are refreshed.       |
| `-Force`           | `Switch`   | No       | Re-downloads driver packages even when a matching cached file already exists.                                                      |
| `-NonInteractive`  | `Switch`   | No       | Skips the Out-GridView picker and processes all matching packages without prompting.                                                |
| `-SkipWifiDrivers` | `Switch`   | No       | Excludes Wi-Fi driver packages (`intel-wifi`, `microsoft-windows-wifi`). Enforced automatically when no imported OS sources exist. |
| `-DownloadOnly`    | `Switch`   | No       | Downloads packages to the cache without expanding them. No `package.json` metadata is written.                                    |

## Examples

```powershell
# Refresh the Dell catalog and open the Out-GridView picker
Update-OSDeployWinPEDrivers -Name 'dell'
```

```powershell
# Refresh Dell and HP catalogs and download all packages without prompting
Update-OSDeployWinPEDrivers -Name 'dell', 'hp' -NonInteractive
```

```powershell
# Refresh all configured sources and open the Out-GridView picker
Update-OSDeployWinPEDrivers
```

```powershell
# Refresh the Dell catalog and download without expanding
Update-OSDeployWinPEDrivers -Name 'dell' -DownloadOnly
```

```powershell
# Refresh all sources, skip Wi-Fi packages, and open the Out-GridView picker
Update-OSDeployWinPEDrivers -SkipWifiDrivers
```

```powershell
# Preview what the Dell non-interactive download would do without executing
Update-OSDeployWinPEDrivers -Name 'dell' -NonInteractive -WhatIf
```

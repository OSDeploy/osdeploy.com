# Get-OSDeployWinPEDrivers

Returns WinPE driver folders from the OSDeployCore library.

| Property | Value                                                        |
|----------|--------------------------------------------------------------|
| Module   | OSDeploy                                                     |
| Platform | Windows 11 (amd64 / arm64)                                  |
| Requires | PowerShell 7.6                                              |
| Output   | `System.IO.DirectoryInfo`                                   |

## Description

Enumerates the `amd64` and `arm64` WinPE driver folders under both the module-managed library (`winpe-drivers`) and the user-managed library (`library\user`) beneath `$env:ProgramData\OSDeployCore\Repository`. Returns a `System.IO.DirectoryInfo` object for each discovered folder.

Results are sorted by `Name` then `FullName`. Use `-Architecture` to limit results to a single architecture, `-SkipWifiDrivers` to exclude wireless-related folders, and `-Interactive` to present an Out-GridView picker before output is returned.

## Syntax

```powershell
Get-OSDeployWinPEDrivers [-Architecture <String>] [-SkipWifiDrivers] [-Interactive]
```

## Parameters

| Parameter          | Type     | Required | Description                                                                                                        |
|--------------------|----------|----------|--------------------------------------------------------------------------------------------------------------------|
| `-Architecture`    | `String` | No       | Limits results to `amd64` or `arm64`. When omitted, both architectures are returned.                              |
| `-SkipWifiDrivers` | `Switch` | No       | Excludes folders whose name contains `wifi` or `wireless` (case-insensitive).                                     |
| `-Interactive`     | `Switch` | No       | Presents discovered folders in an Out-GridView picker. Only selected rows are passed through to output.            |

## Examples

```powershell
# Return all WinPE driver folders for both architectures
Get-OSDeployWinPEDrivers
```

```powershell
# Return only amd64 WinPE driver folders
Get-OSDeployWinPEDrivers -Architecture amd64
```

```powershell
# Return amd64 folders, excluding Wi-Fi and Wireless entries
Get-OSDeployWinPEDrivers -Architecture amd64 -SkipWifiDrivers
```

```powershell
# Open an Out-GridView picker and return only the selected folders
Get-OSDeployWinPEDrivers -Interactive
```

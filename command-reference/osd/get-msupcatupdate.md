# Get-MsUpCatUpdate

Retrieves updates for a specific Windows operating system version from Microsoft Update Catalog

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Searches Microsoft Update Catalog for updates specific to a Windows operating system and build version.

## Syntax

```powershell
Get-MsUpCatUpdate [[-OS] <String>] [[-Arch] <String>] [[-Build] <String>] [[-Category] <String>] [-Insider]
 [-ListAvailable] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OS` | `String` | False | Operating system to search for updates. Valid values are Windows 11, Windows 10, Windows Server, Windows Server 2016, Windows Server 2019, or Windows Server 2022. Default is Windows 11. |
| `-Arch` | `String` | False | Processor architecture filter. Valid values are x64 or x86. Default is x64. |
| `-Build` | `String` | False | Windows build or release ID. Valid values include 22H2, 21H2, 21H1, 20H2, and others. Default is 22H2. |
| `-Category` | `String` | False | No additional description provided. |
| `-Insider` | `SwitchParameter` | False | No additional description provided. |
| `-ListAvailable` | `SwitchParameter` | False | No additional description provided. |

## Examples

### Example
```powershell
Get-MsUpCatUpdate -OS 'Windows 11' -Arch x64 -Build 22H2
Retrieves updates for Windows 11 22H2 x64
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

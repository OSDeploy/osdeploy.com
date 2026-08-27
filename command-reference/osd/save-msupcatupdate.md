# Save-MsUpCatUpdate

Downloads updates from Microsoft Update Catalog for a specific Windows version

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads Microsoft updates for a specific Windows operating system version, architecture, and build from Microsoft Update Catalog to a destination directory.

## Syntax

```powershell
Save-MsUpCatUpdate [[-OS] <String>] [[-Arch] <String>] [[-Build] <String>] [[-Category] <String>]
 [[-Include] <String[]>] [[-DestinationDirectory] <String>] [-Latest] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OS` | `String` | False | Operating system version. Valid values are Windows 10, Windows Server, Windows Server 2016, Windows Server 2019, or Windows Server 2022. Default is Windows 11. |
| `-Arch` | `String` | False | Processor architecture. Valid values are x64 or x86. Default is x64. |
| `-Build` | `String` | False | Windows build or release ID such as 22H2, 21H2, 21H1, 20H2, and others. Default is 22H2. |
| `-Category` | `String` | False | No additional description provided. |
| `-Include` | `String[]` | False | No additional description provided. |
| `-DestinationDirectory` | `String` | False | No additional description provided. |
| `-Latest` | `SwitchParameter` | False | No additional description provided. |

## Examples

### Example
```powershell
Save-MsUpCatUpdate -OS 'Windows 11' -Arch x64 -Build 22H2
Downloads updates for Windows 11 22H2 x64 to the default location
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

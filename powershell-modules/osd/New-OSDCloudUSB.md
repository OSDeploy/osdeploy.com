# New-OSDCloudUSB

Creates resources by using New-OSDCloudUSB.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for New-OSDCloudUSB.

## Syntax

### Workspace (Default)
```powershell
New-OSDCloudUSB [-WorkspacePath <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### fromIsoFile
```powershell
New-OSDCloudUSB -fromIsoFile <FileInfo> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### fromIsoUrl
```powershell
New-OSDCloudUSB -fromIsoUrl <String> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WorkspacePath` | `String` | False | Specifies the value for WorkspacePath. |
| `-fromIsoFile` | `FileInfo` | True | Specifies the value for fromIsoFile. |
| `-fromIsoUrl` | `String` | True | Specifies the value for fromIsoUrl. |

## Examples

### Example
```powershell
-fromIsoFile <fromIsoFile>
Runs New-OSDCloudUSB with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

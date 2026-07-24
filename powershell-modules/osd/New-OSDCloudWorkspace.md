# New-OSDCloudWorkspace

Creates resources by using New-OSDCloudWorkspace.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for New-OSDCloudWorkspace.

## Syntax

### fromTemplate (Default)
```powershell
New-OSDCloudWorkspace [[-WorkspacePath] <String>] [-Public] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### fromUsbDrive
```powershell
New-OSDCloudWorkspace [[-WorkspacePath] <String>] [-fromUsbDrive] [-Public]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### fromIsoUrl
```powershell
New-OSDCloudWorkspace [[-WorkspacePath] <String>] -fromIsoUrl <String> [-Public]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### fromIsoFile
```powershell
New-OSDCloudWorkspace [[-WorkspacePath] <String>] -fromIsoFile <FileInfo> [-Public]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WorkspacePath` | `String` | False | Specifies the value for WorkspacePath. |
| `-fromIsoFile` | `FileInfo` | True | Specifies the value for fromIsoFile. |
| `-fromIsoUrl` | `String` | True | Specifies the value for fromIsoUrl. |
| `-fromUsbDrive` | `SwitchParameter` | True | Indicates whether to enable fromUsbDrive. |
| `-Public` | `SwitchParameter` | False | Indicates whether to enable Public. |

## Examples

### Example
```powershell
-fromIsoFile <fromIsoFile>
Runs New-OSDCloudWorkspace with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

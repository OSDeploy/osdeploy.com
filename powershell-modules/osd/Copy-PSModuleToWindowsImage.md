# Copy-PSModuleToWindowsImage

Copies PowerShell modules to a mounted Windows image

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Copies specified PowerShell modules from the running operating system to a mounted Windows image for offline servicing.

## Syntax

```powershell
Copy-PSModuleToWindowsImage [-Name] <String[]> [-ExecutionPolicy <String>] [-Path <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Name` | `String[]` | True | Name of the PowerShell module(s) to copy. Wildcard patterns are supported. This parameter is mandatory. |
| `-ExecutionPolicy` | `String` | False | Sets the PowerShell Execution Policy in the Windows image. Valid values are Restricted, AllSigned, RemoteSigned, Unrestricted, Bypass, and Undefined. |
| `-Path` | `String[]` | False | Path to the mounted Windows image. If not specified, will use the currently mounted image. |

## Examples

### Example
```powershell
Copy-PSModuleToWindowsImage -Name 'OSD' -Path 'C:\Mount'
Copies the OSD module to the mounted image at C:\\Mount
```

### Example
```powershell
Copy-PSModuleToWindowsImage -Name 'OSD','ActiveDirectory' -ExecutionPolicy Bypass -Path 'C:\Mount'
Copies multiple modules and sets execution policy
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

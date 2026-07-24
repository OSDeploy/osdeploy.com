# Copy-PSModuleToWim

Copies PowerShell modules into an offline Windows image.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Mounts one or more WIM images, copies selected modules into the offline
module path, optionally sets the image execution policy, and saves changes.

## Syntax

```powershell
Copy-PSModuleToWim [[-ExecutionPolicy] <String>] [-ImagePath] <String[]> [[-Index] <UInt32>] [-Name] <String[]>
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ExecutionPolicy` | `String` | False | Optional execution policy to set in the mounted image. |
| `-ImagePath` | `String[]` | True | One or more WIM image file paths to mount and update. |
| `-Index` | `UInt32` | False | Image index to mount from each WIM. Default is 1. |
| `-Name` | `String[]` | True | One or more module names to copy into the image. |

## Examples

### Example
```powershell
Copy-PSModuleToWim -ImagePath 'C:\Media\boot.wim' -Name OSD
Copies the latest installed OSD module into index 1 of boot.wim.
```

### Example
```powershell
Copy-PSModuleToWim -ImagePath 'C:\Media\boot.wim' -Index 2 -Name OSD -ExecutionPolicy RemoteSigned
Copies modules to image index 2 and sets execution policy in the mounted image.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

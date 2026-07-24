# Add-7Zip2BootImage

Adds 7-Zip command-line binaries to a mounted Windows image.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads the latest 7-Zip release assets from GitHub and copies the
extracted binaries into Windows\System32 for the target mount path.

## Syntax

```powershell
Add-7Zip2BootImage [[-MountPath] <String>] [-Use7zr] [-TempTest] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-MountPath` | `String` | False | Mounted Windows image path. If omitted, uses the currently mounted image. |
| `-Use7zr` | `SwitchParameter` | False | Copies 7zr.exe only instead of the full 7z x64 binaries. |
| `-TempTest` | `SwitchParameter` | False | Uses a temporary test path under %TEMP% instead of a mounted image. |

## Examples

### Example
```powershell
Add-7Zip2BootImage -MountPath 'C:\Mount'
Downloads and copies 7-Zip binaries into C:\Mount\Windows\System32.
```

### Example
```powershell
Add-7Zip2BootImage -Use7zr
Adds only 7zr.exe to the detected mounted image.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

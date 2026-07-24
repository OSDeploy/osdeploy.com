# Mount-MyWindowsImage

Mounts MyWindowsImage for servicing.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Mounts MyWindowsImage and prepares it for offline servicing tasks.

## Syntax

```powershell
Mount-MyWindowsImage [-ImagePath] <String[]> [-Index <UInt32>] [-ReadOnly] [-Explorer]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ImagePath` | `String[]` | True | Specifies the ImagePath to use when running Mount-MyWindowsImage. |
| `-Index` | `UInt32` | False | Specifies the Index to use when running Mount-MyWindowsImage. |
| `-ReadOnly` | `SwitchParameter` | False | Specifies the ReadOnly to use when running Mount-MyWindowsImage. |
| `-Explorer` | `SwitchParameter` | False | Specifies the Explorer to use when running Mount-MyWindowsImage. |

## Examples

### Example
```powershell
Demonstrates a common way to run Mount-MyWindowsImage.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

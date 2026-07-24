# Update-MyWindowsImage

Updates MyWindowsImage content.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Installs updates into MyWindowsImage according to the supplied parameters.

## Syntax

```powershell
Update-MyWindowsImage [[-Path] <String[]>] [[-Update] <String>] [-BitsTransfer] [-Force]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | False | Specifies the Path to use when running Update-MyWindowsImage. |
| `-Update` | `String` | False | Specifies the Update to use when running Update-MyWindowsImage. |
| `-BitsTransfer` | `SwitchParameter` | False | Specifies the BitsTransfer to use when running Update-MyWindowsImage. |
| `-Force` | `SwitchParameter` | False | Specifies the Force to use when running Update-MyWindowsImage. |

## Examples

### Example
```powershell
Demonstrates a common way to run Update-MyWindowsImage.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

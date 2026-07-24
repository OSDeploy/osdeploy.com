# Edit-MyWindowsImage

Edits MyWindowsImage content.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Applies modifications to MyWindowsImage in the current servicing workflow.

## Syntax

### Offline (Default)
```powershell
Edit-MyWindowsImage [-Path <String[]>] [-CleanupImage <String>] [-GridRemoveAppxPP] [-RemoveAppxPP <String[]>]
 [-DismountSave] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Online
```powershell
Edit-MyWindowsImage [-Online] [-GridRemoveAppx] [-GridRemoveAppxPP] [-RemoveAppx <String[]>]
 [-RemoveAppxPP <String[]>] [-DismountSave] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | False | Specifies the Path to use when running Edit-MyWindowsImage. |
| `-CleanupImage` | `String` | False | Specifies the CleanupImage to use when running Edit-MyWindowsImage. |
| `-Online` | `SwitchParameter` | True | Specifies the Online to use when running Edit-MyWindowsImage. |
| `-GridRemoveAppx` | `SwitchParameter` | False | Specifies the GridRemoveAppx to use when running Edit-MyWindowsImage. |
| `-GridRemoveAppxPP` | `SwitchParameter` | False | Specifies the GridRemoveAppxPP to use when running Edit-MyWindowsImage. |
| `-RemoveAppx` | `String[]` | False | Specifies the RemoveAppx to use when running Edit-MyWindowsImage. |
| `-RemoveAppxPP` | `String[]` | False | Specifies the RemoveAppxPP to use when running Edit-MyWindowsImage. |
| `-DismountSave` | `SwitchParameter` | False | Specifies the DismountSave to use when running Edit-MyWindowsImage. |

## Examples

### Example
```powershell
Demonstrates a common way to run Edit-MyWindowsImage.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Dismount-MyWindowsImage

Dismounts MyWindowsImage and finalizes changes.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Commits or discards changes to MyWindowsImage and then unmounts the image.

## Syntax

### DismountDiscard (Default)
```powershell
Dismount-MyWindowsImage [-Path <String[]>] [-Discard] [-ProgressAction <ActionPreference>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### DismountSave
```powershell
Dismount-MyWindowsImage [-Path <String[]>] [-Save] [-ProgressAction <ActionPreference>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | False | Specifies the Path to use when running Dismount-MyWindowsImage. |
| `-Discard` | `SwitchParameter` | True | Specifies the Discard to use when running Dismount-MyWindowsImage. |
| `-Save` | `SwitchParameter` | True | Specifies the Save to use when running Dismount-MyWindowsImage. |
| `-WhatIf` | `SwitchParameter` | False | Shows what would happen if the cmdlet runs. The cmdlet is not run. |
| `-Confirm` | `SwitchParameter` | False | Prompts you for confirmation before running the cmdlet. |

## Examples

### Example
```powershell
Demonstrates a common way to run Dismount-MyWindowsImage.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Block-AdminUser

Blocks execution if the current user has Administrator rights

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that the current user does not have Administrator rights.
If admin rights are detected, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-AdminUser [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-AdminUser
Halts execution if the user is running as Administrator
```

### Example
```powershell
Block-AdminUser -Warn
Shows a warning but continues execution even if user is Administrator
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

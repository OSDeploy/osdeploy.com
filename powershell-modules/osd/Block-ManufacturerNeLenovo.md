# Block-ManufacturerNeLenovo

Blocks execution if the computer is not manufactured by Lenovo

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that the computer is manufactured by Lenovo.
If the manufacturer is not Lenovo, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-ManufacturerNeLenovo [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-ManufacturerNeLenovo
Halts execution if the computer is not a Lenovo device
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

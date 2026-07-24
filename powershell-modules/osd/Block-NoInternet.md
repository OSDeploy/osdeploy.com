# Block-NoInternet

Blocks execution if internet connectivity is not available

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates internet connectivity by testing connections to multiple well-known URLs.
If connectivity cannot be established, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-NoInternet [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-NoInternet
Halts execution if internet connectivity is not available
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

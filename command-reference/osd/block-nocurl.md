# Block-NoCurl

Blocks execution if curl.exe is not available

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that curl.exe is available in the Windows System32 directory.
If curl.exe is not found, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-NoCurl [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-NoCurl
Halts execution if curl.exe is not available
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

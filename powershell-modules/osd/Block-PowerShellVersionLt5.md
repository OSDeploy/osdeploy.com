# Block-PowerShellVersionLt5

Blocks execution if PowerShell version is less than 5

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that PowerShell version 5 or greater is running.
If the version is less than 5, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-PowerShellVersionLt5 [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-PowerShellVersionLt5
Halts execution if PowerShell version is less than 5
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

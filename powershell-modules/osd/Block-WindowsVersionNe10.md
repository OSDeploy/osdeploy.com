# Block-WindowsVersionNe10

Blocks execution if Windows major version is not 10

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that the operating system is Windows with major version 10 or greater.
If the major version is not 10, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-WindowsVersionNe10 [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-WindowsVersionNe10
Halts execution if Windows major version is not 10
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

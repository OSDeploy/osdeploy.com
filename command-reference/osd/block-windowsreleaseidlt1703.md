# Block-WindowsReleaseIdLt1703

Blocks execution if Windows ReleaseId is less than 1703

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that Windows ReleaseId is 1703 or greater.
If the ReleaseId is less than 1703, writes a warning and breaks execution unless the -Warn parameter is specified.

## Syntax

```powershell
Block-WindowsReleaseIdLt1703 [-Warn] [-Pause] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Warn` | `SwitchParameter` | False | Shows a warning but continues execution instead of breaking |
| `-Pause` | `SwitchParameter` | False | Pauses and displays a message before continuing execution |

## Examples

### Example
```powershell
Block-WindowsReleaseIdLt1703
Halts execution if Windows ReleaseId is less than 1703
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

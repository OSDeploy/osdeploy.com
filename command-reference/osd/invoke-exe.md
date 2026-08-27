# Invoke-Exe

Runs an external command.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Calls an external command outside of the PowerShell script and logs the output.

## Syntax

```powershell
Invoke-Exe [-Executable] <String> [[-Arguments] <Object>] [-ProgressAction <ActionPreference>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Executable` | `String` | True | Executable that needs to be run. |
| `-Arguments` | `Object` | False | Arguments for the executable. Default is NULL. |
| `-WhatIf` | `SwitchParameter` | False | Shows what would happen if the cmdlet runs. The cmdlet is not run. |
| `-Confirm` | `SwitchParameter` | False | Prompts you for confirmation before running the cmdlet. |

## Examples

### Example
```powershell
Invoke-Exe dir c:\
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

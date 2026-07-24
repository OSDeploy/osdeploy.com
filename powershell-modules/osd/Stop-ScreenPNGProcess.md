# Stop-ScreenPNGProcess

Stops the background screenshot capture process

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Terminates the background PowerShell process that is capturing screenshots and clears related global variables.

## Syntax

```powershell
Stop-ScreenPNGProcess [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Stop-ScreenPNGProcess
Stops the background screenshot process
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

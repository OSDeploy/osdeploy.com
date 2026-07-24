# Get-ComObjMicrosoftUpdateAutoUpdate

Gets Microsoft Update automatic update settings through COM.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates the Microsoft.Update.AutoUpdate COM object and returns its Settings
object for inspection of current automatic update configuration.

## Syntax

```powershell
Get-ComObjMicrosoftUpdateAutoUpdate [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-ComObjMicrosoftUpdateAutoUpdate
Returns Windows Update automatic update settings from the local device.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

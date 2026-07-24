# Get-ComObjMicrosoftUpdateServiceManager

Gets Windows Update service registration details through COM.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates the Microsoft.Update.ServiceManager COM object and returns the
registered update Services collection from the local device.

## Syntax

```powershell
Get-ComObjMicrosoftUpdateServiceManager [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-ComObjMicrosoftUpdateServiceManager
Returns registered Windows Update services from the local system.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

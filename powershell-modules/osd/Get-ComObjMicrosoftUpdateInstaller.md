# Get-ComObjMicrosoftUpdateInstaller

Creates and returns the Microsoft Update installer COM object.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Instantiates the Microsoft.Update.Installer COM object so callers can query
or manage update installation behavior through the Windows Update API.

## Syntax

```powershell
Get-ComObjMicrosoftUpdateInstaller [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-ComObjMicrosoftUpdateInstaller
Returns a Microsoft.Update.Installer COM object instance.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

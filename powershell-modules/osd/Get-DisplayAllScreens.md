# Get-DisplayAllScreens

Returns all display screens on the system

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Enumerates all display screens connected to the system using Windows Forms assembly and sorts them by device name.

## Syntax

```powershell
Get-DisplayAllScreens [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-DisplayAllScreens
Returns all connected displays sorted by device name
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-DisplayPrimaryScaling

Returns the DPI scaling percentage of the primary display

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Calculates the current DPI scaling percentage of the primary monitor by comparing logical and physical screen heights.

## Syntax

```powershell
Get-DisplayPrimaryScaling [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-DisplayPrimaryScaling
Returns the DPI scaling percentage (e.g., 100, 125, 150)
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

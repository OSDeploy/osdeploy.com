# Get-DisplayPrimaryBitmapSize

Returns the primary display bitmap size accounting for DPI scaling

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Calculates the primary display monitor size in pixels, adjusted for the current DPI scaling percentage.

## Syntax

```powershell
Get-DisplayPrimaryBitmapSize [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-DisplayPrimaryBitmapSize
Returns the scaled primary display dimensions
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

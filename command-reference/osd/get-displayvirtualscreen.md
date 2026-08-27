# Get-DisplayVirtualScreen

Returns the virtual screen dimensions covering all displays

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns the overall virtual screen information that encompasses all connected displays, including width, height, and position coordinates.

## Syntax

```powershell
Get-DisplayVirtualScreen [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-DisplayVirtualScreen
Returns the combined dimensions of all displays
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

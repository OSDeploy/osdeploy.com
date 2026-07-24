# Get-MyDellBios

Returns the latest compatible Dell BIOS update for the current system.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Detects the current Dell system SKU, filters the cached Dell BIOS catalog for
compatible entries, and returns the newest matching BIOS update object.
This
function only returns data when it is run on Dell hardware.

## Syntax

```powershell
Get-MyDellBios [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-MyDellBios
Returns the newest compatible Dell BIOS update object for the current Dell device.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

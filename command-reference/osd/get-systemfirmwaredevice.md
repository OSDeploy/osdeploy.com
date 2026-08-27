# Get-SystemFirmwareDevice

Returns the system firmware device

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the system firmware device by querying Win32_PnpEntity for the System Firmware class GUID.

## Syntax

```powershell
Get-SystemFirmwareDevice [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-SystemFirmwareDevice
Returns the system firmware device information
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

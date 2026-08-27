# Get-SystemFirmwareResource

Returns the GUID of the system firmware resource

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the system firmware device and extracts GUID values directly from
its PNP Device ID for use with Microsoft Update Catalog queries.

## Syntax

```powershell
Get-SystemFirmwareResource [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-SystemFirmwareResource
Returns the firmware resource GUID
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

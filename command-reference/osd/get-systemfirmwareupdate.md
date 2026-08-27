# Get-SystemFirmwareUpdate

Retrieves the latest system firmware update from Microsoft Update Catalog

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Searches Microsoft Update Catalog directly for the latest system firmware
update available for the current computer firmware resource GUID.

## Syntax

```powershell
Get-SystemFirmwareUpdate [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-SystemFirmwareUpdate
Returns the latest available firmware update
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

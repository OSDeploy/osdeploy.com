# Convert-PNPDeviceIDtoGuid

Extracts GUID values from a PNP Device ID string.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses a regular expression to locate and return GUID values embedded in a
Plug and Play device identifier.
Accepts input directly or from the
pipeline.

## Syntax

```powershell
Convert-PNPDeviceIDtoGuid [-PNPDeviceID] <String> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-PNPDeviceID` | `String` | True | PNP device ID string to search for GUID values. |

## Examples

### Example
```powershell
Convert-PNPDeviceIDtoGuid -PNPDeviceID 'USB\\VID_1234&PID_5678\\{12345678-1234-1234-1234-1234567890AB}'
Returns the GUID found in the PNP device ID.
```

### Example
```powershell
'USB\\VID_1234&PID_5678\\{12345678-1234-1234-1234-1234567890AB}' | Convert-PNPDeviceIDtoGuid
Returns the GUID found in the piped PNP device ID.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

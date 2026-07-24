# Get-HPDriverPackLatest

Gets the latest available HP driver pack for a platform.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Checks supported OS releases for the target platform, searches from newest
to oldest release for Windows 11 and then Windows 10, and returns the first
matching Driver Pack entry found in the HPIA SoftPaq catalog.

## Syntax

```powershell
Get-HPDriverPackLatest [[-Platform] <String>] [-URL] [-download] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Platform` | `String` | False | HP platform ID to query. If not provided, the local baseboard product ID is used. |
| `-URL` | `SwitchParameter` | False | Returns only the full download URL for the discovered driver pack. |
| `-download` | `SwitchParameter` | False | Downloads the discovered driver pack to C:\Drivers using Save-WebFile. |

## Examples

### Example
```powershell
Get-HPDriverPackLatest
Returns the latest driver pack metadata for the local platform.
```

### Example
```powershell
Get-HPDriverPackLatest -Platform 83B2 -URL
Returns only the driver pack URL for platform 83B2.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

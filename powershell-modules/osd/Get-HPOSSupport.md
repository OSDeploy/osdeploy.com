# Get-HPOSSupport

Gets supported Windows releases for an HP platform from the HPIA catalog.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads and parses the HP platform catalog and returns operating system
support data for a specified platform or the local device platform.
Optional
switches can return only the latest supported OS values.

## Syntax

```powershell
Get-HPOSSupport [[-Platform] <String>] [-Latest] [-MaxOS] [-MaxOSVer] [-MaxOSNum]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Platform` | `String` | False | HP platform ID to query. If not provided, the local baseboard product ID is used. |
| `-Latest` | `SwitchParameter` | False | Returns a combined string containing the latest supported OS description and release ID. |
| `-MaxOS` | `SwitchParameter` | False | Returns the latest supported OS family as Win10 or Win11. |
| `-MaxOSVer` | `SwitchParameter` | False | Returns the latest supported OS release ID value. |
| `-MaxOSNum` | `SwitchParameter` | False | Returns the latest supported OS major version number as 10.0 or 11.0. |

## Examples

### Example
```powershell
Get-HPOSSupport
Returns all supported OS entries for the local platform.
```

### Example
```powershell
Get-HPOSSupport -Platform 83B2 -MaxOSVer
Returns the maximum supported release ID for platform 83B2.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-HPSoftpaqListLatest

Gets the latest HPIA SoftPaq list for an HP platform.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves the latest supported OS information for a platform, downloads the
corresponding HPIA reference CAB, and returns the SoftPaq update list from
the extracted XML metadata.

## Syntax

```powershell
Get-HPSoftpaqListLatest [[-Platform] <String>] [-SystemInfo] [-MaxOSVer] [-MaxOSNum]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Platform` | `String` | False | HP platform ID to query. If not provided, the local baseboard product ID is used. |
| `-SystemInfo` | `SwitchParameter` | False | Returns system information from the HPIA XML instead of the SoftPaq list. |
| `-MaxOSVer` | `SwitchParameter` | False | Reserved switch parameter in this function signature. |
| `-MaxOSNum` | `SwitchParameter` | False | Reserved switch parameter in this function signature. |

## Examples

### Example
```powershell
Get-HPSoftpaqListLatest
Returns the latest SoftPaq list for the local platform.
```

### Example
```powershell
Get-HPSoftpaqListLatest -Platform 83B2 -SystemInfo
Returns system information metadata for platform 83B2.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

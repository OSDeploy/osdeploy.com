# Get-HPSoftPaqItems

Gets HPIA SoftPaq items for a specific HP platform and OS release.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates that the requested operating system and release are supported by
the target platform, downloads the matching HPIA CAB metadata file, and
returns the SoftPaq update entries from the extracted XML.

## Syntax

```powershell
Get-HPSoftPaqItems [[-Platform] <String>] [-osver] <String> [-os] <String> [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Platform` | `String` | False | HP platform ID to query. If not provided, the local baseboard product ID is used. |
| `-osver` | `String` | True | Operating system release ID value to query, such as 23H2. |
| `-os` | `String` | True | Operating system major version number to query. Valid values are 10.0 and 11.0. |

## Examples

### Example
```powershell
Get-HPSoftPaqItems -osver 23H2 -os 11.0
Returns SoftPaq items for Windows 11 23H2 on the local platform.
```

### Example
```powershell
Get-HPSoftPaqItems -Platform 83B2 -osver 22H2 -os 10.0
Returns SoftPaq items for Windows 10 22H2 on platform 83B2.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-OSDCoreLicense

Returns a single Recast Core license object.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Reads .license2 files from the Recast Software license directory, parses the
JSON payload, validates expected fields, removes duplicates, and returns one
selected license object.
Selection precedence is:
1) PreferredEmail exact match (when provided)
2) Any non-empty email that is not support@recastsoftware.com
3) support@recastsoftware.com fallback
4) Empty or null email
Expired licenses are still eligible for selection.

## Syntax

```powershell
Get-OSDCoreLicense [[-Path] <String>] [[-PreferredEmail] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | False | The directory path to search for .license2 files. |
| `-PreferredEmail` | `String` | False | Optional preferred email value used to prioritize license selection when multiple license files exist. Comparison is case-insensitive. |

## Examples

### Example
```powershell
Get-OSDCoreLicense
Returns a selected validated license object from ProgramData\Recast Software\Licenses.
```

### Example
```powershell
Get-OSDCoreLicense -Path 'D:\Licenses'
Returns a selected validated license object from a custom directory.
```

### Example
```powershell
Get-OSDCoreLicense -PreferredEmail 'david@segura.org'
Prefers a license with the specified email when available.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

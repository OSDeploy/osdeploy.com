# Get-ComObjects

Lists registered COM ProgIDs from the local machine registry.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Enumerates COM object ProgIDs under HKLM:\Software\Classes that map to a CLSID.
Use -ListAll to return the full list, or -Filter to return matching entries.

## Syntax

### FilterByName
```powershell
Get-ComObjects -Filter <String> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### ListAllComObjects
```powershell
Get-ComObjects [-ListAll] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Filter` | `String` | True | Wildcard pattern used to match ProgID names (for example, Microsoft.Update.*). |
| `-ListAll` | `SwitchParameter` | True | Returns all discovered COM ProgIDs without applying a name filter. |

## Examples

### Example
```powershell
Get-ComObjects -ListAll
Returns all COM ProgIDs that contain a CLSID registration.
```

### Example
```powershell
Get-ComObjects -Filter 'Microsoft.Update.*'
Returns only COM ProgIDs that match the specified wildcard pattern.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

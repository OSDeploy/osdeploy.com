# Invoke-MSCatalogParseDate

Parses a date string from Microsoft Update Catalog format

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Converts a date string in MM/DD/YYYY format (as returned by Microsoft Update Catalog) into a PowerShell DateTime object.

## Syntax

```powershell
Invoke-MSCatalogParseDate [[-DateString] <String>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DateString` | `String` | False | Date string in MM/DD/YYYY format to parse |

## Examples

### Example
```powershell
Invoke-MSCatalogParseDate -DateString "01/15/2025"
Returns a DateTime object for January 15, 2025
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

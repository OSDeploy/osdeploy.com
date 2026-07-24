# Get-OSDCoreDriverPackCatalogSurface

Retrieves the Microsoft Surface driver pack catalog, enriching entries from live download pages.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Loads the bundled surface.json catalog as the offline base.
For entries that include an
UpdatePage URL, the function scrapes the corresponding Microsoft download page to find the
newest available MSI and updates FileName, Url, and ReleaseDate accordingly.
Results are cached in $env:TEMP so subsequent calls within the same session skip network
requests.
Falls back to base JSON values when a page cannot be reached.

## Syntax

```powershell
Get-OSDCoreDriverPackCatalogSurface [[-LocalDriverPackCatalog] <String>] [-Force] [-LocalOnly]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-LocalDriverPackCatalog` | `String` | False | Path to the local fallback Surface catalog JSON file. |
| `-Force` | `SwitchParameter` | False | Forces bypass of the temp cache and rebuilds the enriched catalog from the local Surface catalog. |
| `-LocalOnly` | `SwitchParameter` | False | Uses only local catalog values and skips connectivity probing and all live UpdatePage checks. |

## Examples

### Example
```powershell
Get-OSDCoreDriverPackCatalogSurface
```

Returns all Surface driver pack entries, with live URLs where available.

### Example
```powershell
Get-OSDCoreDriverPackCatalogSurface -Verbose
```

Returns all Surface driver pack entries with verbose progress output.

### Example
```powershell
Get-OSDCoreDriverPackCatalogSurface -Force
```

Bypasses the temp cache and rebuilds the enriched catalog.

### Example
```powershell
Get-OSDCoreDriverPackCatalogSurface -LocalOnly
```

Processes only local catalog values without any live network checks.

# Get-OSDCoreDriverPackCatalogHP

Downloads and parses the HP driver pack catalog for Windows 11.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the latest HP Client Driver Pack Catalog from HP's cloud repository,
extracts and parses it to create a catalog of available Windows 11 driver packs.
Falls back to offline catalog if download fails.

## Syntax

```powershell
Get-OSDCoreDriverPackCatalogHP [[-LocalDriverPackCatalog] <String>] [[-OemDriverPackCatalog] <String>] [-Force]
 [-LocalOnly] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-LocalDriverPackCatalog` | `String` | False | Path to the local fallback HP catalog XML file. |
| `-OemDriverPackCatalog` | `String` | False | URL to the online HP driver pack catalog CAB file. |
| `-Force` | `SwitchParameter` | False | Forces download and rebuild of the temporary online catalog even when a cached temp catalog file already exists. |
| `-LocalOnly` | `SwitchParameter` | False | Uses only local catalog values and skips online catalog download/extraction. |

## Examples

### Example
```powershell
Get-OSDCoreDriverPackCatalogHP
```

Retrieves the HP driver pack catalog for Windows 11.

### Example
```powershell
Get-OSDCoreDriverPackCatalogHP -Force
```

Forces a refresh of the HP driver pack catalog by downloading the latest version
from HP's server, bypassing any cached copies.

### Example
```powershell
Get-OSDCoreDriverPackCatalogHP -LocalOnly
```

Processes only local catalog values without any online download checks.

# Get-OSDCoreDriverPackCatalogDell

Downloads and parses the Dell driver pack catalog for Windows 11.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the latest Dell DriverPackCatalog.cab from Dell's download site,
extracts and parses it to create a catalog of available Windows 11 driver packs.
If online retrieval fails, the function falls back to the bundled local catalog.

## Syntax

```powershell
Get-OSDCoreDriverPackCatalogDell [[-LocalDriverPackCatalog] <String>] [[-OemDriverPackCatalog] <String>]
 [-Force] [-LocalOnly] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-LocalDriverPackCatalog` | `String` | False | Path to the local fallback Dell catalog XML file. This file is used when the online catalog cannot be downloaded or extracted. |
| `-OemDriverPackCatalog` | `String` | False | URL to the online Dell DriverPack catalog CAB file. |
| `-Force` | `SwitchParameter` | False | Forces download and rebuild of the temporary online catalog even when a cached temp catalog file already exists. |
| `-LocalOnly` | `SwitchParameter` | False | Uses only local catalog values and skips online catalog download/extraction. |

## Examples

### Example
```powershell
Get-OSDCoreDriverPackCatalogDell
```

Retrieves the Dell driver pack catalog for Windows 11.

### Example
```powershell
Get-OSDCoreDriverPackCatalogDell -Force
```

Forces a fresh online download of the Dell catalog before parsing.

### Example
```powershell
Get-OSDCoreDriverPackCatalogDell -LocalDriverPackCatalog 'C:\Catalogs\dell.xml'
```

Uses a custom local fallback catalog path.

### Example
```powershell
Get-OSDCoreDriverPackCatalogDell -LocalOnly
```

Processes only local catalog values without any online download checks.

# Get-OSDCoreDriverPackCatalogLenovo

Downloads and parses the Lenovo driver pack catalog for Windows 11.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the latest Lenovo SCCM driver pack catalog from Lenovo's download site,
parses the XML to create a catalog of available Windows 11 driver packs.
Falls back to offline catalog if download fails.

## Syntax

```powershell
Get-OSDCoreDriverPackCatalogLenovo [[-LocalDriverPackCatalog] <String>] [[-OemDriverPackCatalog] <String>]
 [-Force] [-LocalOnly] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-LocalDriverPackCatalog` | `String` | False | Path to the local fallback Lenovo catalog XML file. |
| `-OemDriverPackCatalog` | `String` | False | URL to the online Lenovo driver pack catalog XML file. |
| `-Force` | `SwitchParameter` | False | Forces download and rebuild of the temporary online catalog even when a cached temp catalog file already exists. |
| `-LocalOnly` | `SwitchParameter` | False | Uses only local catalog values and skips online catalog download. |

## Examples

### Example
```powershell
Get-OSDCoreDriverPackCatalogLenovo
```

Retrieves the Lenovo driver pack catalog for Windows 11.

### Example
```powershell
Get-OSDCoreDriverPackCatalogLenovo -Force
```

Forces a fresh online download of the Lenovo catalog before parsing.

### Example
```powershell
Get-OSDCoreDriverPackCatalogLenovo -LocalOnly
```

Processes only local catalog values without any online download checks.

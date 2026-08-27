# Get-HPSystemCatalog

Converts the HP Client Catalog for Microsoft System Center Product to a PowerShell Object

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Converts the HP Client Catalog for Microsoft System Center Product to a PowerShell Object
Requires Internet Access to download HpCatalogForSms.latest.cab

## Syntax

```powershell
Get-HPSystemCatalog [[-DownloadPath] <String>] [-Compatible] [[-Component] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Specifies a download path for matching results displayed in Out-GridView |
| `-Compatible` | `SwitchParameter` | False | Limits the results to match the current system |
| `-Component` | `String` | False | Limits the results to a specified component |

## Examples

### Example
```powershell
Get-HPSystemCatalog
Don't do this, you will get an almost endless list
```

### Example
```powershell
$Results = Get-HPSystemCatalog
Yes do this.  Save it in a Variable
```

### Example
```powershell
Get-HPSystemCatalog -Component BIOS | Out-GridView
Displays all the HP BIOS updates in GridView
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

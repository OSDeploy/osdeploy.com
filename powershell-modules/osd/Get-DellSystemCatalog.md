# Get-DellSystemCatalog

Builds the Dell System Catalog

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Builds the Dell System Catalog

## Syntax

```powershell
Get-DellSystemCatalog [-Compatible] [[-Component] <String>] [[-DownloadPath] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Compatible` | `SwitchParameter` | False | Limits the results to match the current system |
| `-Component` | `String` | False | Limits the results to a specified component |
| `-DownloadPath` | `String` | False | Specifies a download path for matching results displayed in Out-GridView |

## Examples

### Example
```powershell
Get-DellSystemCatalog
Don't do this, you will get an almost endless list
```

### Example
```powershell
$Result = Get-DellSystemCatalog
Yes do this.  Save it in a Variable
```

### Example
```powershell
Get-DellSystemCatalog -Component BIOS | Out-GridView
Displays all the Dell BIOS Updates in GridView
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

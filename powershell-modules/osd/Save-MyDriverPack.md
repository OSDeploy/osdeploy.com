# Save-MyDriverPack

Downloads and optionally expands the driver pack for the current computer

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads the matching driver pack from OSDCloud for the current or specified computer.
Can optionally extract and expand the driver pack after download.

## Syntax

```powershell
Save-MyDriverPack [[-DownloadPath] <String>] [-Expand] [[-Manufacturer] <String>] [[-Product] <String>]
 [[-Guid] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Directory where the driver pack will be saved. Default is C:\Drivers |
| `-Expand` | `SwitchParameter` | False | Automatically expands the driver pack archive after download |
| `-Manufacturer` | `String` | False | Computer manufacturer. Default is auto-detected |
| `-Product` | `String` | False | Computer product model. Default is auto-detected |
| `-Guid` | `String` | False | GUID of a specific driver pack to download |

## Examples

### Example
```powershell
Save-MyDriverPack
Downloads the driver pack for the current computer to C:\Drivers
```

### Example
```powershell
Save-MyDriverPack -DownloadPath 'D:\DriverPacks' -Expand
Downloads and expands the driver pack to D:\DriverPacks
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

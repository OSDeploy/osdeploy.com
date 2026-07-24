# Invoke-HPIAOfflineSync

Creates and synchronizes an offline HPIA repository for the local HP platform.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Builds a local repository using HPCMSL commands, applies platform and OS
filters, and downloads selected update content for offline use.
Logs are
written to C:\Windows\TEMP\osdcloud-logs\HPIAOfflineSync.log.

## Syntax

```powershell
Invoke-HPIAOfflineSync [[-Category] <Object>] [[-OS] <Object>] [[-Release] <Object>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Category` | `Object` | False | Update category filter for repository content. Valid values are All, BIOS, Driver, Software, Firmware, and UWPPack. |
| `-OS` | `Object` | False | Operating system filter passed to Add-RepositoryFilter, such as win11. |
| `-Release` | `Object` | False | Operating system release filter passed to Add-RepositoryFilter, such as 23H2. |

## Examples

### Example
```powershell
Invoke-HPIAOfflineSync
Creates an offline repository for the local platform using default Driver, win11, and 23H2 filters.
```

### Example
```powershell
Invoke-HPIAOfflineSync -Category BIOS -OS win10 -Release 22H2
Creates an offline repository filtered to Windows 10 22H2 BIOS content.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

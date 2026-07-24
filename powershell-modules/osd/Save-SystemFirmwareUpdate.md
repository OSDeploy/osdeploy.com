# Save-SystemFirmwareUpdate

Downloads and extracts the latest system firmware update.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Finds the latest applicable system firmware update from Microsoft Update
Catalog, downloads the package, and extracts its contents to a destination
directory.

## Syntax

```powershell
Save-SystemFirmwareUpdate [[-DestinationDirectory] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DestinationDirectory` | `String` | False | Directory where the firmware update package will be downloaded and extracted. |

## Examples

### Example
```powershell
Save-SystemFirmwareUpdate
Downloads and extracts the latest firmware update to the default temp path.
```

### Example
```powershell
Save-SystemFirmwareUpdate -DestinationDirectory C:\Drivers\SystemFirmware
Downloads and extracts the latest firmware update to C:\Drivers\SystemFirmware.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

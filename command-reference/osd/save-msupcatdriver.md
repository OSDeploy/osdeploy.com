# Save-MsUpCatDriver

Downloads driver updates from Microsoft Update Catalog

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Searches Microsoft Update Catalog for drivers matching specified hardware IDs or Plug and Play device classes and downloads them to a destination directory.

## Syntax

### ByPNPClass (Default)
```powershell
Save-MsUpCatDriver [-DestinationDirectory <String>] [-PNPClass <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### ByHardwareID
```powershell
Save-MsUpCatDriver [-DestinationDirectory <String>] [-HardwareID <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DestinationDirectory` | `String` | False | Directory where downloaded drivers will be saved |
| `-HardwareID` | `String[]` | False | One or more hardware IDs to search for drivers (ParameterSet: ByHardwareID) |
| `-PNPClass` | `String` | False | Plug and Play device class to search for drivers. Valid values are DiskDrive, Display, Net, SCSIAdapter, SecurityDevices, or USB. (ParameterSet: ByPNPClass) |

## Examples

### Example
```powershell
Save-MsUpCatDriver -DestinationDirectory 'C:\Drivers' -PNPClass 'Net'
Downloads network driver updates to C:\Drivers
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

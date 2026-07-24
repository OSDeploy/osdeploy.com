# Save-MyDellBiosFlash64W

Downloads and extracts the Dell Flash64W BIOS utility.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads the Flash64W support package referenced by the current compatible
Dell BIOS update and extracts Flash64W.exe to the specified folder.
This is
primarily used to support BIOS flashing from WinPE x64 environments on Dell
hardware.

## Syntax

```powershell
Save-MyDellBiosFlash64W [[-DownloadPath] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Specifies the directory where the Flash64W package should be downloaded and extracted. The default location is the current user's temporary folder. |

## Examples

### Example
```powershell
Save-MyDellBiosFlash64W
Downloads and extracts Flash64W.exe to the default temporary folder.
```

### Example
```powershell
Save-MyDellBiosFlash64W -DownloadPath 'C:\OSDCloud\Firmware'
Downloads and extracts Flash64W.exe to C:\OSDCloud\Firmware.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

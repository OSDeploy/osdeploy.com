# Save-MyDellBios

Downloads the latest compatible Dell BIOS update to a local folder.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves the current system's compatible Dell BIOS update and downloads the
BIOS package to the specified folder when it is not already present.
This
function only operates on Dell hardware and returns the existing or newly
downloaded BIOS file when successful.

## Syntax

```powershell
Save-MyDellBios [[-DownloadPath] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Specifies the directory where the Dell BIOS update should be stored. The default location is the current user's temporary folder. |

## Examples

### Example
```powershell
Save-MyDellBios
Downloads the compatible Dell BIOS update to the default temporary folder.
```

### Example
```powershell
Save-MyDellBios -DownloadPath 'C:\OSDCloud\Firmware'
Downloads the compatible Dell BIOS update to C:\OSDCloud\Firmware.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

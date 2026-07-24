# New-WindowsAdkISO

Creates an ISO file from a bootable media directory using ADK

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates an ISO file from a bootable media directory using Windows Assessment and Deployment Kit (ADK) tools.

## Syntax

```powershell
New-WindowsAdkISO [-MediaPath] <FileInfo> [-isoFileName] <String> [-isoLabel] <String>
 [[-IsoDirectory] <FileInfo>] [[-WindowsAdkRoot] <FileInfo>] [-OpenExplorer]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-MediaPath` | `FileInfo` | True | Path to the directory containing the bootable media |
| `-isoFileName` | `String` | True | Filename for the output ISO file |
| `-isoLabel` | `String` | True | Label for the ISO volume (limited to 16 characters) |
| `-IsoDirectory` | `FileInfo` | False | No additional description provided. |
| `-WindowsAdkRoot` | `FileInfo` | False | Path to the Windows ADK root directory (optional if installed in default location) |
| `-OpenExplorer` | `SwitchParameter` | False | Opens Windows Explorer to the parent directory of the ISO File |

## Examples

### Example
```powershell
New-WindowsAdkISO -MediaPath 'C:\\Media' -isoFileName 'boot.iso' -isoLabel 'BootMedia'\n    Creates an ISO file from the bootable media
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# New-AdkISO

Creates an ISO file from a bootable media directory using ADK tools

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates an ISO file from a bootable media directory.
Requires the Windows Assessment and Deployment Kit (ADK) to be installed.

## Syntax

```powershell
New-AdkISO [[-WindowsAdkRoot] <String>] [-MediaPath] <String> [-isoFileName] <String> [-isoLabel] <String>
 [-OpenExplorer] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WindowsAdkRoot` | `String` | False | Path to Windows ADK root directory. Optional if ADK is in default location. |
| `-MediaPath` | `String` | True | Path to the directory containing the bootable media |
| `-isoFileName` | `String` | True | Filename of the output ISO file |
| `-isoLabel` | `String` | True | Label of the ISO (limited to 16 characters) |
| `-OpenExplorer` | `SwitchParameter` | False | Switch to open Windows Explorer to the parent directory of the ISO file after creation |

## Examples

### Example
```powershell
New-AdkISO -MediaPath 'C:\BootMedia' -isoFileName 'WinPE.iso' -isoLabel 'WinPE'
Creates an ISO file from the bootable media
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Convert-EsdToIso

Converts an ESD file into an ISO image.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Expands and exports required images from an ESD into a temporary media
folder, then creates an ISO using Convert-FolderToIso.

## Syntax

```powershell
Convert-EsdToIso [-esdFullName] <String> [[-isoFullName] <String>] [[-isoLabel] <String>] [-noPrompt] [-Demo]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-esdFullName` | `String` | True | Full path to the source ESD file. |
| `-isoFullName` | `String` | False | Destination ISO file path. If omitted, an ISO is created beside the ESD. |
| `-isoLabel` | `String` | False | ISO volume label. Must be 1 to 16 characters. |
| `-noPrompt` | `SwitchParameter` | False | Uses no-prompt UEFI boot image behavior when creating the ISO. |
| `-Demo` | `SwitchParameter` | False | Shows conversion actions without exporting images. |

## Examples

### Example
```powershell
Convert-EsdToIso -esdFullName 'C:\Media\install.esd'
Converts install.esd to an ISO in the same directory.
```

### Example
```powershell
Convert-EsdToIso -esdFullName 'C:\Media\install.esd' -isoFullName 'C:\ISO\Custom.iso' -isoLabel 'CustomISO' -noPrompt
Converts the ESD to a custom-labeled ISO at the specified path.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

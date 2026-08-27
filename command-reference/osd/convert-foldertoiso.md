# Convert-FolderToIso

Creates an ISO file from a source folder.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses Windows ADK oscdimg to create a standard or bootable ISO from a folder.
The function validates required boot files when present and supports optional
no-prompt UEFI boot media generation.

## Syntax

```powershell
Convert-FolderToIso [-folderFullName] <String> [-isoFullName <String>] [-isoLabel <String>] [-noPrompt]
 [-WindowsAdkRoot <FileInfo>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-folderFullName` | `String` | True | Source folder path to convert into an ISO. |
| `-isoFullName` | `String` | False | Destination ISO file path. If omitted, an ISO is created beside the source folder using the folder name. |
| `-isoLabel` | `String` | False | ISO volume label. Must be 1 to 16 characters. |
| `-noPrompt` | `SwitchParameter` | False | Uses efisys_noprompt.bin when available for UEFI boot media. |
| `-WindowsAdkRoot` | `FileInfo` | False | Optional Windows ADK root path used to resolve oscdimg.exe. |

## Examples

### Example
```powershell
Convert-FolderToIso -folderFullName 'C:\OSD\Media'
Creates C:\OSD\Media.iso from the specified folder.
```

### Example
```powershell
Convert-FolderToIso -folderFullName 'C:\OSD\Media' -isoFullName 'C:\ISO\Custom.iso' -isoLabel 'CustomISO' -noPrompt
Creates a bootable ISO at the specified destination with a custom label.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

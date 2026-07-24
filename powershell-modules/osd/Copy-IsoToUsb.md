# Copy-IsoToUsb

Creates a bootable USB drive from a Windows ISO.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Formats a selected USB disk, mounts the ISO, and copies installation files
to the USB volume.
Supports FAT32 or NTFS, optional bootsect execution, and
optional splitting of large install.wim files.

## Syntax

```powershell
Copy-IsoToUsb [-ISOFile] <String> [-MakeBootable] [-NTFS] [-SplitWim] [[-USBLabel] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ISOFile` | `String` | True | Full path to the ISO file to mount and copy. |
| `-MakeBootable` | `SwitchParameter` | False | Runs bootsect.exe against the USB drive after formatting. |
| `-NTFS` | `SwitchParameter` | False | Formats the USB drive as NTFS instead of FAT32. |
| `-SplitWim` | `SwitchParameter` | False | Forces splitting install.wim into .swm files during copy. |
| `-USBLabel` | `String` | False | File system label assigned to the USB drive. |

## Examples

### Example
```powershell
Copy-IsoToUsb -ISOFile 'C:\Temp\Win11.iso' -MakeBootable -USBLabel WIN11
Creates a bootable USB and copies the ISO contents.
```

### Example
```powershell
Copy-IsoToUsb -ISOFile 'C:\Temp\Win11.iso' -NTFS -USBLabel WIN11NTFS
Creates an NTFS-formatted USB and copies the ISO contents.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

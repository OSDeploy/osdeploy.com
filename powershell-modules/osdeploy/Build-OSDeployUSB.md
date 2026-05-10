# Build-OSDeployUSB

Creates a new bootable OSDeploy USB drive from an OSDeployCore BootImage build.

| Property | Value                                                                    |
|----------|--------------------------------------------------------------------------|
| Module   | OSDeploy                                                                 |
| Platform | Windows 10 or later (amd64 / arm64)                                     |
| Requires | Run as Administrator, a completed BootImage build, a USB drive ≥ 7 GB   |

## Description

Prepares a USB drive for use as an OSDeploy bootable device. The function:

- Prompts for selection of a completed BootImage from `%ProgramData%\OSDeployCore\boot-media`
- Prompts for the specific bootmedia folder (`bootmedia` or `bootmedia-ca2023`)
- Selects a suitable USB disk (7 GB–2 TB) via an interactive picker
- Clears, partitions (MBR), and formats the selected disk:
  - 4 GB FAT32 active boot partition (labeled with `-BootLabel`)
  - NTFS remaining-space data partition (labeled with `-DataLabel`)
- Copies the selected BootMedia to the FAT32 partition

{% hint style="danger" %}
This function clears and repartitions the selected USB disk. All existing data on the disk will be destroyed. Use `Update-OSDeployUSB` to refresh an existing OSDeploy USB without reformatting.
{% endhint %}

## Syntax

```powershell
Build-OSDeployUSB [-BootLabel <String>] [-DataLabel <String>]
```

## Parameters

| Parameter     | Type     | Required | Description                                                                       |
|---------------|----------|----------|-----------------------------------------------------------------------------------|
| `-BootLabel`  | `String` | No       | Volume label for the FAT32 boot partition. Max 11 characters. Default: `USB-WinPE`. |
| `-DataLabel`  | `String` | No       | Volume label for the NTFS data partition. Max 32 characters. Default: `USB-DATA`.  |

## Examples

```powershell
# Create a new OSDeploy USB using default partition labels
Build-OSDeployUSB
```

```powershell
# Create a new OSDeploy USB with custom partition labels
Build-OSDeployUSB -BootLabel 'OSDEPLOY' -DataLabel 'OSD-DATA'
```

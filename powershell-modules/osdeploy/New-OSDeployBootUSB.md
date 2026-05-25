# New-OSDeployBootUSB

Creates a new bootable OSDeploy USB drive from an OSDeployCore BootImage build.

| Property | Value                                                                   |
|----------|-------------------------------------------------------------------------|
| Module   | OSDeploy                                                                |
| Platform | Windows 11 (amd64 / arm64)                                             |
| Requires | Run as Administrator, a completed BootImage build, a USB drive ≥ 7 GB  |

## Description

Prepares a USB drive for use as an OSDeploy bootable device. The function:

- Prompts for selection of a completed BootImage from `%ProgramData%\OSDeployCore\boot`
- Prompts for the specific bootmedia folder (`bootmedia` or `bootmedia_ca2023`)
- Selects a suitable USB disk (7 GB–2 TB) via an interactive picker
- Clears, partitions (MBR), and formats the selected disk:
  - 4 GB FAT32 active boot partition (labeled with `-BootLabel`)
  - NTFS remaining-space data partition (labeled with `-DataLabel`)
- Copies the selected BootMedia to the FAT32 partition

{% hint style="danger" %}
This function clears and repartitions the selected USB disk. All existing data on the disk will be destroyed. Use `Update-OSDeployBootUSB` to refresh an existing OSDeploy USB without reformatting.
{% endhint %}

## Syntax

```powershell
New-OSDeployBootUSB [-BootLabel <String>] [-DataLabel <String>]
```

## Parameters

| Parameter    | Type     | Required | Description                                                                        |
|--------------|----------|----------|------------------------------------------------------------------------------------|
| `-BootLabel` | `String` | No       | Volume label for the FAT32 boot partition. Max 11 characters. Default: `OSDEPLOY`. |
| `-DataLabel` | `String` | No       | Volume label for the NTFS data partition. Max 32 characters. Default: `OSDCloud`.  |

## Examples

```powershell
# Create a new OSDeploy USB using default partition labels
New-OSDeployBootUSB
```

```powershell
# Create a new OSDeploy USB with custom partition labels
New-OSDeployBootUSB -BootLabel 'WinPE' -DataLabel 'WinPE-Data'
```

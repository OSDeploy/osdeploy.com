# Update-OSDeployBootUSB

Updates an existing OSDeploy USB drive with new BootMedia from an OSDeployCore BootImage build.

| Property | Value                                                                                               |
|----------|-----------------------------------------------------------------------------------------------------|
| Module   | OSDeploy                                                                                            |
| Platform | Windows 11 (amd64 / arm64)                                                                         |
| Requires | Run as Administrator, a completed BootImage build, a USB drive previously created by `New-OSDeployBootUSB` |

## Description

Refreshes the BootMedia files on one or more existing OSDeploy USB drives without reformatting or repartitioning. The function:

- Prompts for selection of a completed BootImage from `%ProgramData%\OSDeployCore\boot`
- Prompts for the specific bootmedia folder (`bootmedia` or `bootmedia_ca2023`)
- Locates all connected USB volumes whose label matches `-BootLabel` (default: `OSDEPLOY`)
- Copies the selected media to each matching USB volume using `robocopy`
- Writes a `BootMedia.json` file to each updated volume

No partitioning or formatting is performed. Use `New-OSDeployBootUSB` to create a new bootable USB drive.

## Syntax

```powershell
Update-OSDeployBootUSB [-BootLabel <String>] [-DataLabel <String>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter    | Type     | Required | Description                                                                                              |
|--------------|----------|----------|----------------------------------------------------------------------------------------------------------|
| `-BootLabel` | `String` | No       | Volume label used to identify USB boot partitions to update. Max 11 characters. Default: `OSDEPLOY`.    |
| `-DataLabel` | `String` | No       | Volume label used to identify the USB data partition when copying ESD files. Max 32 characters. Default: `OSDCloud`. |

## Examples

```powershell
# Update all connected USB volumes labeled 'OSDEPLOY'
Update-OSDeployBootUSB
```

```powershell
# Update all connected USB volumes labeled 'WinPE'
Update-OSDeployBootUSB -BootLabel 'WinPE'
```

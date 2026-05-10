# Update-OSDeployUSB

Updates an existing OSDeploy USB drive with new BootMedia from an OSDeployCore BootImage build.

| Property | Value                                                                                      |
|----------|--------------------------------------------------------------------------------------------|
| Module   | OSDeploy                                                                                   |
| Platform | Windows 10 or later (amd64 / arm64)                                                       |
| Requires | Run as Administrator, a completed BootImage build, a USB drive previously created by `New-OSDeployUSB` |

## Description

Refreshes the BootMedia files on one or more existing OSDeploy USB drives without reformatting or repartitioning. The function:

- Prompts for selection of a completed BootImage from `%ProgramData%\OSDeployCore\boot-media`
- Prompts for the specific bootmedia folder (`bootmedia` or `bootmedia-ca2023`)
- Locates all connected USB volumes whose label matches `-BootLabel` (default: `USB-WinPE`)
- Copies the selected media to each matching USB volume using `robocopy`
- Writes a `BootMedia.json` file to each updated volume

No partitioning or formatting is performed. Use `New-OSDeployUSB` to create a new bootable USB drive.

## Syntax

```powershell
Update-OSDeployUSB [-BootLabel <String>]
```

## Parameters

| Parameter    | Type     | Required | Description                                                                                        |
|--------------|----------|----------|----------------------------------------------------------------------------------------------------|
| `-BootLabel` | `String` | No       | Volume label used to identify USB boot partitions to update. Max 11 characters. Default: `USB-WinPE`. |

## Examples

```powershell
# Update all connected USB volumes labeled 'USB-WinPE'
Update-OSDeployUSB
```

```powershell
# Update all connected USB volumes labeled 'OSDEPLOY'
Update-OSDeployUSB -BootLabel 'OSDEPLOY'
```

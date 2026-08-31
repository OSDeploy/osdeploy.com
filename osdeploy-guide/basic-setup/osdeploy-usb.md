---
description: Create a two-partition OSDCloud USB drive from completed OSDeploy Boot media.
---

# Create a new OSDeploy USB

Use `New-OSDeployBootUSB` to prepare a USB disk from a completed OSDeploy Boot build. The function clears the selected disk, creates boot and data partitions, copies the selected WinPE media, and adds local OSDCloud content when it is available and fits.

{% hint style="info" %}
Use `Update-OSDeployBootUSB` when an existing OSDeploy USB drive only needs refreshed boot media or OSDCloud content and its partition layout does not need to change.
{% endhint %}

## Create the USB Drive

{% stepper %}
{% step %}
### Confirm the Requirements

Run the function on a workstation that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../requirements/powershell-modules.md)
* Administrator rights
* `curl.exe` available in `PATH`
* `Out-GridView` for selecting the build and media folder
* A completed [OSDeploy Boot](osdeploy-boot.md) build under `C:\ProgramData\OSDeployCore\boot`
* An online USB disk larger than 7 GB and smaller than 2000 GB

{% hint style="danger" %}
`New-OSDeployBootUSB` removes every partition and all data from the selected USB disk. Verify the disk number, model, and size before approving the clear operation. Use `Update-OSDeployBootUSB` when an existing OSDeploy USB only needs refreshed content.
{% endhint %}
{% endstep %}

{% step %}
### Create the Partitions and Copy the Media

Open an elevated PowerShell 7.6 session and run:

```powershell
New-OSDeployBootUSB
```

Select an eligible USB disk by number, choose a completed build, and select its `bootmedia` or `bootmedia_ca2023` directory. The default command uses this layout:

| Setting                | Default                             |
| ---------------------- | ----------------------------------- |
| Partition style        | MBR                                 |
| Boot partition         | 4 GB, FAT32, active                 |
| Boot label             | `OSDEPLOY`                          |
| Data partition         | Remaining space, NTFS               |
| Data label             | `OSDCloud`                          |
| Boot media             | Interactive selection               |
| Local OSDCloud content | Copy when present and space permits |

{% hint style="info" %}
The 4 GB FAT32 boot partition is fixed. Store larger local operating-system content on the NTFS data partition rather than expanding the boot partition.
{% endhint %}

The function lists eligible USB disks and prompts for a disk number. It then sets the machine-wide Explorer `NoDriveTypeAutorun` policy to `0xFF`, opens selectors for the completed build and `bootmedia` or `bootmedia_ca2023`, clears and converts the disk to MBR, creates both partitions, and copies the selected media. Local content under `C:\ProgramData\OSDeployCore\OSDCloud` is copied to the NTFS partition when it exists and available space is sufficient.

{% hint style="warning" %}
Missing or oversized local OSDCloud content does not prevent creation of the bootable USB. The function writes a message and leaves the NTFS data partition without that optional content.
{% endhint %}

{% hint style="warning" %}
`-WhatIf` does not provide a read-only preview. It still changes the machine-wide AutoRun policy, and an already empty RAW or GPT disk can be initialized or converted to MBR before partition creation is suppressed.
{% endhint %}
{% endstep %}

{% step %}
### Verify the USB Drive

Set `$DiskNumber` to the selected USB disk number, then inspect its partition style, partitions, labels, and file systems:

```powershell
$DiskNumber = 3

Get-Disk -Number $DiskNumber |
	Select-Object Number, FriendlyName, PartitionStyle, NumberOfPartitions

Get-Partition -DiskNumber $DiskNumber |
	Get-Volume |
	Select-Object DriveLetter, FileSystemLabel, FileSystem, Size, SizeRemaining
```

Confirm that the disk uses MBR and contains the `OSDEPLOY` FAT32 boot partition and `OSDCloud` NTFS data partition. Confirm that the boot partition contains the selected WinPE media.
{% endstep %}
{% endstepper %}

For labels, selection rules, copy behavior, `WhatIf`, and returned objects, see [New-OSDeployBootUSB](../advanced/new-osdeploybootusb.md). To refresh prepared media without repartitioning, see [Update-OSDeployBootUSB](../advanced/update-osdeploybootusb.md). For compact syntax, see the [New-OSDeployBootUSB command reference](../../command-reference/osdeploy/new-osdeploybootusb.md) and [Update-OSDeployBootUSB command reference](../../command-reference/osdeploy/update-osdeploybootusb.md).

---
description: Create a two-partition OSDCloud USB drive from completed OSDeploy Boot media.
---

# Create an OSDCloud USB

Use `New-OSDeployBootUSB` to prepare a USB disk from a completed OSDeploy Boot build. The function clears the selected disk, creates boot and data partitions, copies the selected WinPE media, and adds local OSDCloud content when it is available and fits.

## Requirements

Run the function on a workstation that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../module-setup/README.md)
* Administrator rights
* `curl.exe` available in `PATH`
* `Out-GridView` for selecting the build and media folder
* A completed [OSDeploy Boot](../build-boot-image/README.md) build under `C:\ProgramData\OSDeployCore\boot`
* An online USB disk larger than 7 GiB and smaller than 2000 GiB

{% hint style="danger" %}
`New-OSDeployBootUSB` removes every partition and all data from the selected USB disk. Verify the disk number, model, and size before approving the clear operation. Use `Update-OSDeployBootUSB` when an existing OSDeploy USB only needs refreshed content.
{% endhint %}

## Basic Usage

Open an elevated PowerShell 7.6 session and run:

```powershell
New-OSDeployBootUSB
```

Select an eligible USB disk by number, choose a completed build, and select its `bootmedia` or `bootmedia_ca2023` directory. The default command uses this layout:

| Setting | Default |
| --- | --- |
| Partition style | MBR |
| Boot partition | 4 GB, FAT32, active |
| Boot label | `OSDEPLOY` |
| Data partition | Remaining space, NTFS |
| Data label | `OSDCloud` |
| Boot media | Interactive selection |
| Local OSDCloud content | Copy when present and space permits |

{% hint style="info" %}
The 4 GB FAT32 boot partition is fixed. Store larger local operating-system content on the NTFS data partition rather than expanding the boot partition.
{% endhint %}

## How It Works

`New-OSDeployBootUSB` performs these actions:

1. Verifies Windows, PowerShell, `curl.exe`, and administrator requirements.
2. Lists online USB disks strictly larger than 7 GiB and strictly smaller than 2000 GiB, then prompts for a disk number until a valid selection is entered.
3. Sets the machine-wide Explorer `NoDriveTypeAutorun` policy to `0xFF`.
4. Opens a single-selection picker for completed builds, followed by another picker for `bootmedia` or `bootmedia_ca2023`.
5. Clears existing partitions when present and converts the empty disk to MBR when required.
6. Creates and formats the active 4 GB FAT32 boot partition and the remaining-space NTFS data partition.
7. Copies the selected boot-media tree to the FAT32 partition.
8. Checks `C:\ProgramData\OSDeployCore\OSDCloud` and copies that content to the NTFS partition when it exists and available space is sufficient.
9. Returns the refreshed Windows Storage disk object after preparation.

{% hint style="warning" %}
Missing or oversized local OSDCloud content does not prevent creation of the bootable USB. The function writes a message and leaves the NTFS data partition without that optional content.
{% endhint %}

{% hint style="warning" %}
`-WhatIf` does not provide a read-only preview. It still changes the machine-wide AutoRun policy, and an already empty RAW or GPT disk can be initialized or converted to MBR before partition creation is suppressed.
{% endhint %}

For labels, selection rules, copy behavior, `WhatIf`, and returned objects, see [New-OSDeployBootUSB](new-osdeploybootusb.md). To refresh prepared media without repartitioning, see [Update-OSDeployBootUSB](update-osdeploybootusb.md). For compact syntax, see the [New-OSDeployBootUSB command reference](../../command-reference/osdeploy/new-osdeploybootusb.md) and [Update-OSDeployBootUSB command reference](../../command-reference/osdeploy/update-osdeploybootusb.md).

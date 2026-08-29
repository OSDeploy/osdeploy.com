---
description: >-
  Create a two-partition OSDeploy USB drive from a completed OSDeploy Boot
  build.
---

# New-OSDeployBootUSB

`New-OSDeployBootUSB` prepares a USB disk with a 4 GB active FAT32 boot partition and an NTFS data partition. Use the command to select completed OSDeploy boot media, assign partition labels, copy boot files, and optionally copy local OSDCloud content.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

The workstation must also have:

* A completed OSDeploy Boot build under `C:\ProgramData\OSDeployCore\boot`.
* `Out-GridView` for selecting the build and boot-media folder.
* An online USB disk larger than 7 GiB and smaller than 2000 GiB.
* A working Windows Storage provider.

See [Module Setup](../module-setup/) to install OSDeploy and [OSDeploy Boot](../build-boot-image/) to create completed boot media.

{% hint style="danger" %}
The function removes every partition and all data from the selected USB disk. Confirm the disk number, model, and size before continuing. Use `Update-OSDeployBootUSB` when an existing OSDeploy USB only needs refreshed media.
{% endhint %}

## Partition Guidance

The partition sizes and file systems are fixed. Only their labels can be changed.

| Partition | Size            | File system   | Default label | Purpose                                                              |
| --------- | --------------- | ------------- | ------------- | -------------------------------------------------------------------- |
| Boot      | 4 GB            | FAT32, active | `OSDEPLOY`    | Stores the selected `bootmedia` or `bootmedia_ca2023` tree.          |
| Data      | Remaining space | NTFS          | `OSDCloud`    | Stores local OSDeploy Core OSDCloud content when it exists and fits. |

Use short, recognizable labels. An empty label is valid, but it makes the partitions harder to identify in later workflows.

## Parameters

All parameters are optional. The function has one parameter set and does not accept pipeline input.

| Parameter    | Type             | Default     | Accepted values and behavior                                                                                                                                                             |
| ------------ | ---------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-BootLabel` | `String`         | `OSDEPLOY`  | Use from 0 through 11 characters for the FAT32 boot-partition label.                                                                                                                     |
| `-DataLabel` | `String`         | `OSDCloud`  | Use from 0 through 32 characters for the NTFS data-partition label.                                                                                                                      |
| `-WhatIf`    | Common parameter | Not enabled | Run discovery and selection, report gated disk and copy operations, and return before partition creation. The AutoRun policy change and empty-disk MBR conversion are not gated.         |
| `-Confirm`   | Common parameter | Not enabled | Request confirmation for each gated clear, partition, and copy operation. The function declares `ConfirmImpact` as `High`, so the effective prompts also depend on `$ConfirmPreference`. |

## Examples

### Create a USB with the default labels

Select an eligible USB disk, a completed build, and one of its boot-media folders. The function creates partitions labeled `OSDEPLOY` and `OSDCloud`:

```powershell
New-OSDeployBootUSB
```

### Use custom partition labels

Use labels that distinguish the boot and data partitions from other removable media:

```powershell
New-OSDeployBootUSB `
	-BootLabel 'WinPE' `
	-DataLabel 'WinPE-Data'
```

### Request confirmation for gated operations

Use `-Confirm` to request approval before each operation controlled by `ShouldProcess`:

```powershell
New-OSDeployBootUSB -Confirm
```

Clearing an existing layout, creating the new partitions, and each applicable file copy are separate confirmation points.

### Preview the gated operations

Use `-WhatIf` to run the selectors and display the clear or partition action that would be performed:

```powershell
New-OSDeployBootUSB -WhatIf
```

{% hint style="warning" %}
`-WhatIf` is not a read-only preview. The function still sets the machine-wide `NoDriveTypeAutorun` policy to `0xFF`. If the selected disk is already empty, it also initializes a RAW disk as MBR or converts a GPT disk to MBR before the partition preview.
{% endhint %}

### Capture the returned disk

The success stream can contain the pre-operation disk snapshot and native `robocopy.exe` text in addition to the final disk. Capture all output, then select the last `MSFT_Disk` CIM instance:

```powershell
$output = @(New-OSDeployBootUSB)

$disk = $output |
	Where-Object { $_.CimClass.CimClassName -eq 'MSFT_Disk' } |
	Select-Object -Last 1

$disk | Format-List Number, FriendlyName, PartitionStyle, SizeGB
```

## USB Disk Selection

The function gets online disks whose Windows Storage bus type is `USB`. Disks with no media are excluded. An eligible disk must be strictly larger than 7 GiB and strictly smaller than 2000 GiB.

Eligible disks are displayed in disk-number order with their bus type, media type, size, friendly name, model, partition style, and partition count. Enter a listed disk number. The prompt repeats until a valid number is entered; this workflow does not provide a skip choice.

After selection, the function gets the same USB disk again and writes a projected snapshot to the success stream before making changes.

## Boot Media Selection

After selecting the USB disk, the function applies this sequence:

1. Set `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\NoDriveTypeAutorun` to `0xFF`.
2. Search the OSDeploy Core `boot` directory for immediate child directories containing a `bootmedia` or `bootmedia_ca2023` subdirectory.
3. Sort completed builds by last-modified time, newest first, and open a single-selection `Out-GridView` picker.
4. Find immediate `bootmedia` and `bootmedia_ca2023` directories in the selected build, sort them by name and full path, and open a second single-selection `Out-GridView` picker.

The function writes a warning and returns when no completed build exists, either picker is canceled, or no supported boot-media folder is available. The AutoRun policy has already been changed when either media picker is canceled.

## Disk Preparation

Disk preparation occurs after all selections are complete:

1. When the disk contains partitions, request approval and run `Clear-Disk -RemoveData -RemoveOEM`.
2. Get the disk again and require it to have zero partitions. Write a warning and return when the cleared state cannot be verified.
3. Initialize a RAW disk as MBR, or convert an empty GPT disk to MBR.
4. Request approval and create an active 4 GB FAT32 partition using `-BootLabel`.
5. Create an NTFS partition from all remaining space using `-DataLabel`.

The clear and partition operations are gated by `ShouldProcess`. RAW initialization and empty GPT-to-MBR conversion are not gated.

## Content Copy

When the selected boot-media source and FAT32 destination both exist, the function uses `robocopy.exe` to copy the complete source tree to the root of the boot partition. The copy is gated by `ShouldProcess`.

The function then checks `C:\ProgramData\OSDeployCore\OSDCloud`. When that source exists, it recursively totals the source-file sizes and compares the result with free space on the NTFS partition.

| Condition                                     | Behavior                                                                          |
| --------------------------------------------- | --------------------------------------------------------------------------------- |
| OSDCloud source does not exist                | Write an informational message and skip the optional copy.                        |
| Free space cannot be determined               | Write a warning and skip the optional copy.                                       |
| Source content is larger than available space | Write the required and available sizes in a warning, then skip the optional copy. |
| Source content fits                           | Request approval and copy it to `OSDCloud` on the NTFS partition.                 |

Skipped optional OSDCloud content does not make USB creation fail. Native `robocopy.exe` standard output is written to the success stream during each copy.

## WhatIf Behavior

`-WhatIf` still performs the environment checks, USB disk prompt, both `Out-GridView` selections, pre-operation disk snapshot, and AutoRun policy change.

For a disk with existing partitions, the clear operation is reported but not performed, and the function returns. For an already empty disk, RAW initialization or GPT-to-MBR conversion can occur before partition creation is reported and skipped. In either case, no final refreshed disk object is returned from the normal completion path.

## Output

On normal completion, the function returns the refreshed `Microsoft.Management.Infrastructure.CimInstance` for the selected `MSFT_Disk`. `Get-OSDeployDisk` adds `MediaType` and `SizeGB` to the Storage provider properties.

| Property             | Description                                                         |
| -------------------- | ------------------------------------------------------------------- |
| `Number`             | Windows disk number selected by the user.                           |
| `FriendlyName`       | Device name reported by the Windows Storage provider.               |
| `BusType`            | Storage bus type; the returned disk is `USB`.                       |
| `PartitionStyle`     | Final partition style; successful preparation uses `MBR`.           |
| `NumberOfPartitions` | Partition count reported after preparation.                         |
| `Size`               | Disk capacity in bytes.                                             |
| `SizeGB`             | Disk capacity converted to an integer number of GiB.                |
| `MediaType`          | Media type enriched from the matching physical disk when available. |

The success stream is heterogeneous. Before preparation, it includes a projected disk snapshot. During copies, it can also include `System.String` records from `robocopy.exe`. Early cancellation, failed clear verification, and `-WhatIf` return no final disk object.

See [Create an OSDCloud USB](./) for the workflow overview or the [New-OSDeployBootUSB command reference](../../command-reference/osdeploy/new-osdeploybootusb.md) for compact syntax and parameter definitions.

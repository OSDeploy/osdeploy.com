---
description: Refresh existing OSDeploy USB volumes from a selected standard or CA 2023 boot-media tree.
---

# Update-OSDeployBootUSB

`Update-OSDeployBootUSB` copies a selected OSDeploy Boot build to existing USB volumes without partitioning or formatting their disks. Use the command to update every connected USB boot volume with a matching label, write build metadata, and optionally refresh OSDCloud content on matching data volumes.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

The workstation must also have:

* A completed OSDeploy Boot build under `C:\ProgramData\OSDeployCore\boot`.
* `Out-GridView` for selecting the build and boot-media folder.
* One or more online USB volumes with the expected boot or data label.
* A working Windows Storage provider.

See [Module Setup](../requirements/powershell-modules.md) to install OSDeploy and [OSDeploy Boot](../basic/build-osdeployboot.md) to create completed boot media.

{% hint style="warning" %}
The function updates every connected USB volume whose label matches `-BootLabel` or `-DataLabel`. Disconnect unrelated USB storage that uses the same labels before running the command.
{% endhint %}

## Update Guidance

Use `Update-OSDeployBootUSB` for a prepared USB drive whose partition layout does not need to change. Use [`New-OSDeployBootUSB`](new-osdeploybootusb.md) to clear a disk and create the OSDeploy boot and data partitions.

The update copies and overwrites source files but does not mirror the source tree. Files that exist only on the destination remain in place.

| Content                | Matching label        | Destination                                      | Behavior                                                                     |
| ---------------------- | --------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------- |
| Selected BootMedia     | `OSDEPLOY` by default | Root of every matching USB volume                | Copy the complete source tree and write `BootMedia.json`.                    |
| Local OSDCloud content | `OSDCloud` by default | `OSDCloud` directory on each matching USB volume | Check free space, prompt separately for each volume, and copy when approved. |

## Parameters

All parameters are optional. The function has one parameter set and does not accept pipeline input.

| Parameter    | Type             | Default     | Accepted values and behavior                                                                                                                                                                   |
| ------------ | ---------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-BootLabel` | `String`         | `OSDEPLOY`  | Use from 0 through 11 characters. Every connected USB volume whose file-system label equals this value is updated.                                                                             |
| `-DataLabel` | `String`         | `OSDCloud`  | Use from 0 through 32 characters. Every matching USB volume is considered for the optional OSDCloud copy.                                                                                      |
| `-WhatIf`    | Common parameter | Not enabled | Perform prerequisite checks, media selection, volume discovery, and free-space checks. Change the AutoRun policy, report gated writes, and still display the OSDCloud `ShouldContinue` prompt. |
| `-Confirm`   | Common parameter | Not enabled | Request confirmation for each BootMedia copy, metadata write, and approved OSDCloud copy. The function uses the default `Medium` impact, so these prompts do not appear under PowerShell's default `High` preference unless requested. It does not suppress the separate OSDCloud prompt. |

Label comparisons use PowerShell's default case-insensitive equality. An empty string passes parameter validation, but a volume is selected only when its provider-returned label equals that value.

## Examples

### Update USB volumes with the default labels

Select a completed build and boot-media folder, then update every connected USB volume labeled `OSDEPLOY`. When local OSDCloud content exists, the function also evaluates volumes labeled `OSDCloud`:

```powershell
Update-OSDeployBootUSB
```

### Match custom boot and data labels

Update all boot volumes labeled `WinPE` and offer the optional OSDCloud copy for every data volume labeled `WinPE-Data`:

```powershell
Update-OSDeployBootUSB `
	-BootLabel 'WinPE' `
	-DataLabel 'WinPE-Data'
```

### Update only the custom boot label

Use a custom boot label while retaining `OSDCloud` as the data-volume label:

```powershell
Update-OSDeployBootUSB -BootLabel 'WinPE'
```

### Confirm each gated write

Request approval for every BootMedia copy, `BootMedia.json` write, and OSDCloud copy that follows the data-volume prompt:

```powershell
Update-OSDeployBootUSB -Confirm
```

Each matching boot volume has two independent confirmation points: one for its file copy and one for its metadata file.

### Preview the update

Run both media selectors, discover matching volumes, and report the gated file operations without changing USB content:

```powershell
Update-OSDeployBootUSB -WhatIf
```

{% hint style="warning" %}
`-WhatIf` still sets the machine-wide `NoDriveTypeAutorun` policy to `0xFF`. When local OSDCloud content fits on a matching data volume, the function also asks whether to copy it before `ShouldProcess` reports that the copy would occur.
{% endhint %}

### Capture native copy output

Capture the standard-output lines emitted by `robocopy.exe` when a copy runs:

```powershell
$copyOutput = @(Update-OSDeployBootUSB)
$copyOutput | Set-Content -Path '.\Update-OSDeployBootUSB.log'
```

The function does not return a status object. No success-stream output is returned when no copy runs or `robocopy.exe` emits no standard output.

## Boot Media Selection

The function selects source content before discovering USB volumes:

1. Search `C:\ProgramData\OSDeployCore\boot` for immediate child directories containing a `bootmedia` or `bootmedia_ca2023` subdirectory.
2. Sort completed builds by last-modified time, newest first, and open a single-selection `Out-GridView` picker.
3. Find immediate `bootmedia` and `bootmedia_ca2023` directories in the selected build.
4. Sort those directories by name and full path, then open a second single-selection `Out-GridView` picker.

The function writes a warning and returns when no completed build exists, either picker is canceled, or no supported boot-media folder is selected. Canceling either selector occurs before the AutoRun policy is changed.

After both selections, the function sets this machine-wide policy value without `ShouldProcess` protection:

```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\NoDriveTypeAutorun = 0xFF
```

## Media Architecture

The command can copy either an `amd64` or `arm64` media tree and does not convert its architecture. Select media that matches the firmware architecture of the devices that will boot the USB.

`bootmedia` is the standard media tree. `bootmedia_ca2023` contains CA 2023 Secure Boot files when the build was created from compatible imported WinRE content. The second selector chooses one tree; the function does not merge or synchronize both variants on a boot volume.

## USB Volume Matching

The function uses the Windows Storage provider to get online disks whose bus type is `USB`. It excludes disks that report no media, maps their partition access paths to volumes, and filters the resulting volumes by exact file-system label.

Matching is volume-based, not disk-pair based. A boot volume and data volume do not need to reside on the same USB disk to be processed. Multiple matching volumes are handled in drive-letter order.

When no boot volume matches `-BootLabel`, the function writes a warning and continues to the optional OSDCloud workflow. When no data volume matches `-DataLabel`, it writes a warning and skips the OSDCloud copy.

## BootMedia Update

For every matching boot volume with an accessible drive letter, the function performs two separately gated operations:

1. Copy the selected `bootmedia` or `bootmedia_ca2023` tree to the volume root with `robocopy.exe`.
2. Serialize the selected build object to `BootMedia.json` in the volume root.

`BootMedia.json` records the selected build's `ModifiedTime`, `Name`, and workstation `Path`. The metadata write is independent of the media copy. Declining or suppressing the copy does not automatically suppress the metadata write.

The `robocopy.exe` options include subdirectories, including empty ones, and exclude junction traversal, `$RECYCLE.BIN`, and `System Volume Information`. The command does not use `/mir` or `/purge`, so destination-only files are retained.

If the selected source path no longer exists, the function writes a warning, skips all boot-volume updates, and continues to the optional OSDCloud workflow. A matching volume whose drive-letter path is unavailable is skipped.

## OSDCloud Update

The optional source is `C:\ProgramData\OSDeployCore\OSDCloud`. When it does not exist, the function writes an informational message and skips the data-volume workflow.

For each matching data volume, the function recursively totals the source-file sizes and gets the volume's remaining space.

| Condition                                     | Behavior                                                                                                |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Free space cannot be determined               | Write a warning and skip this volume.                                                                   |
| Source content is larger than available space | Write the required and available sizes in a warning, then skip this volume.                             |
| Source content fits                           | Display a `ShouldContinue` prompt containing the source, destination, file count, size, and free space. |
| User declines the prompt                      | Write an informational message and skip this volume.                                                    |
| User approves the prompt                      | Apply `ShouldProcess`, then copy the source tree to the volume's `OSDCloud` directory.                  |

The `ShouldContinue` prompt is separate from `-Confirm` and appears once for each data volume that has determinable, sufficient free space.

## WhatIf and Confirmation

`-WhatIf` still performs all prerequisite checks, opens both `Out-GridView` selectors, sets the AutoRun policy, discovers matching volumes, checks paths, totals OSDCloud source files, and checks free space.

BootMedia copies and `BootMedia.json` writes are reported but suppressed. For each data volume with enough space, the function still displays the `ShouldContinue` prompt. Approving it causes `ShouldProcess` to report and suppress the OSDCloud copy; declining it skips that report. No USB content is changed by the gated operations.

Without `-Confirm`, BootMedia and metadata writes proceed without a PowerShell confirmation prompt under the default confirmation preference. The OSDCloud `ShouldContinue` prompt still appears once for every eligible data volume and cannot be bypassed with `-Confirm:$false`. With `-Confirm`, approved OSDCloud copies receive both the data-volume prompt and the subsequent `ShouldProcess` prompt.

## Output

The function returns no structured result. When a BootMedia or OSDCloud copy runs, native `robocopy.exe` standard-output lines are written to the success stream as `System.String` values.

No success-stream objects are returned when selection is canceled, no copy runs, all optional copies are declined or skipped, or `-WhatIf` suppresses every copy. Host messages, warnings, confirmation prompts, and `WhatIf` messages are not structured return objects.

See [Create an OSDCloud USB](../basic/new-osdeploybootusb.md) for the workflow overview or the [Update-OSDeployBootUSB command reference](../../command-reference/osdeploy/update-osdeploybootusb.md) for compact syntax and parameter definitions.

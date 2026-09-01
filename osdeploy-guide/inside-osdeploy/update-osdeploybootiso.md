---
description: >-
  Rebuild standard and CA 2023 ISO files from the existing media trees in a
  completed OSDeploy Boot build.
---

# The OSDeploy Boot ISO

`Update-OSDeployBootISO` recreates bootable ISO files for a selected OSDeploy Boot build. Use it after changing files in an existing media tree or updating saved OSDCloud profiles when `boot.wim` does not need to be serviced again.

The function rebuilds `bootmedia.iso` from `bootmedia` and, when present, `bootmedia_ca2023.iso` from `bootmedia_ca2023`. It does not mount, service, export, or replace `boot.wim`.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

The workstation must also have:

* The [OSDeploy module](../requirements/powershell-modules.md).
* The [Windows ADK Deployment Tools](/broken/pages/KKnKou096GC0HYAS6jiH), including `oscdimg.exe`.
* `Out-GridView` for selecting a completed build.
* A completed OSDeploy Boot build under `C:\ProgramData\OSDeployCore\boot` with a `bootmedia` directory.

{% hint style="warning" %}
The function stops when a host requirement is not met, no build is selected, the Windows ADK is unavailable, or the selected build does not contain `bootmedia`. Use [Build-OSDeployBoot](../cmdlets/build-osdeployboot.md) to create a new media tree or service `boot.wim`.
{% endhint %}

## Parameters

The function has no command-specific parameters and does not accept pipeline input. Build selection is always interactive.

| Parameter  | Type             | Default     | Accepted values and behavior                                                                                                                                                  |
| ---------- | ---------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-WhatIf`  | Common parameter | Not enabled | Run requirement checks and build selection, verify the ADK and media paths, populate `$global:BuildMedia`, and report the planned rebuild without calling the ISO build step. |
| `-Confirm` | Common parameter | Not enabled | Request confirmation before the ISO build step replaces the output files. The inherited confirmation preference can also apply to each delegated ISO creation operation.      |

The function declares `ConfirmImpact` as `Medium`. Without `-Confirm`, the rebuild proceeds without a confirmation prompt under PowerShell's default `High` confirmation preference.

## Examples

### Rebuild the ISO files

Select a completed OSDeploy Boot build and recreate every ISO supported by its existing media directories:

```powershell
Update-OSDeployBootISO
```

The standard `bootmedia.iso` is always rebuilt. The CA 2023 ISO is rebuilt only when the selected build contains `bootmedia_ca2023`.

### Request confirmation

Review the selected build path before allowing the output ISO files to be replaced:

```powershell
Update-OSDeployBootISO -Confirm
```

Confirmation can occur at the parent rebuild operation and at delegated ISO creation because both commands support `ShouldProcess`.

### Preview the rebuild

Open the build selector, validate the selected media and ADK, and display the planned rebuild without changing profiles or ISO files:

```powershell
Update-OSDeployBootISO -WhatIf
```

The selected configuration remains available in `$global:BuildMedia` after the preview.

### Inspect the rebuilt files

Run the update, then inspect the ISO files in the selected build directory recorded in the global build state:

```powershell
Update-OSDeployBootISO

Get-ChildItem `
	-LiteralPath $global:BuildMedia.MediaRootPath `
	-Filter '*.iso' |
	Select-Object Name, Length, LastWriteTime
```

`Update-OSDeployBootISO` itself does not return the created files to the pipeline.

## Build Selection

The function initializes the OSDeploy Core paths and searches immediate child directories under:

```
C:\ProgramData\OSDeployCore\boot
```

A directory appears in the selector when it contains either `bootmedia` or `bootmedia_ca2023`. Results are sorted by last-modified time, newest first, and displayed in a single-selection `Out-GridView` picker with these properties:

| Property       | Description                             |
| -------------- | --------------------------------------- |
| `ModifiedTime` | Last-write time of the build directory. |
| `Name`         | Build directory name.                   |
| `Path`         | Full build directory path.              |

Canceling the picker writes a warning and returns without rebuilding an ISO. The same behavior applies when no qualifying build exists.

After selection, the function requires `{selected build}\bootmedia`. A directory that contains only `bootmedia_ca2023` can appear in the selector but cannot be updated because the missing standard media directory causes the function to warn and return.

## Media Selection

The media paths are derived automatically from the selected build:

| Source directory   | Output file            | Condition                                 |
| ------------------ | ---------------------- | ----------------------------------------- |
| `bootmedia`        | `bootmedia.iso`        | Required and always processed.            |
| `bootmedia_ca2023` | `bootmedia_ca2023.iso` | Processed only when the directory exists. |

Both ISO files are written in the selected build root. Existing files with the same names can be replaced by `oscdimg.exe`.

The selected build directory name is used as the ISO volume label. `Update-OSDeployBootISO` does not expose parameters for choosing another source directory, output filename, or volume label.

## OSDCloud Profiles

Before creating each ISO, the shared ISO step checks:

```
C:\ProgramData\OSDeployCore\OSDCloud\Profiles
```

When that source exists, the step copies it into `OSDCloud\Profiles` in the applicable media tree. This makes the current saved OSDCloud profiles available in the ISO without permanently retaining the copied folder in the source media.

The operation order is:

1. Copy the ProgramData profile directory into the standard media tree when the source exists.
2. Create `bootmedia.iso`.
3. Remove `bootmedia\OSDCloud\Profiles` when that destination exists.
4. Repeat the copy, ISO creation, and removal for `bootmedia_ca2023` when present.

{% hint style="warning" %}
The cleanup removes the complete `OSDCloud\Profiles` destination after each ISO attempt, including content that existed there before the command started. Store persistent profiles in `C:\ProgramData\OSDeployCore\OSDCloud\Profiles`, not only inside a build's media tree.
{% endhint %}

The copy and cleanup are performed by the shared ISO build step and are not separately gated by `ShouldProcess`. The parent `-WhatIf` gate prevents the entire step from running.

## ISO Creation

The delegated ISO command searches the installed ADK root for `oscdimg.exe` in this order:

1. `Deployment Tools\amd64\Oscdimg\oscdimg.exe`
2. `Deployment Tools\arm64\Oscdimg\oscdimg.exe`

This order selects the available ISO creation tool; it does not change the architecture of the existing media tree.

The command then selects the first available UEFI boot-sector file in this order:

1. `EFI\Microsoft\Boot\efisys_noprompt.bin` in the media tree.
2. `EFI\Microsoft\Boot\efisys.bin` in the media tree.
3. `efisys_noprompt.bin` beside `oscdimg.exe`.
4. `efisys.bin` beside `oscdimg.exe`.

When neither `oscdimg.exe` nor a boot-sector file can be found, the delegated command writes a non-terminating error and returns without creating that ISO. The profile cleanup still runs after the delegated command returns.

The ISO uses UDF 1.02, UEFI boot data, file optimization, and support for media larger than a standard CD. It is a UEFI-only ISO; the helper does not add a BIOS or legacy boot entry.

If `oscdimg.exe` exits with a nonzero code, the function writes a non-terminating error. It does not restore an ISO that was replaced or partially written.

## WhatIf Behavior

`-WhatIf` still performs these operations:

* Validate Windows, PowerShell, `curl.exe`, and administrator access.
* Initialize OSDeploy Core paths.
* Discover completed builds and open the `Out-GridView` selector.
* Verify the Windows ADK installation.
* Require the selected build's `bootmedia` directory.
* Detect the optional `bootmedia_ca2023` directory.
* Populate `$global:BuildMedia` and display the resolved paths.

The parent `ShouldProcess` operation then reports `Rebuild BootMedia ISO files` and does not call the shared ISO step. No profiles are copied or removed, `oscdimg.exe` is not started, and existing ISO files are not changed.

## Global Build State

The function replaces `$global:BuildMedia` with an ordered dictionary used by the shared ISO step:

| Property        | Description                                                         |
| --------------- | ------------------------------------------------------------------- |
| `AdkRootPath`   | Installed Windows ADK path.                                         |
| `MediaIsoLabel` | Selected build directory name used as the volume label.             |
| `MediaPath`     | Full path to the required `bootmedia` directory.                    |
| `MediaPathEX`   | Full path to `bootmedia_ca2023`, or `$null` when it does not exist. |
| `MediaRootPath` | Full path to the selected build directory and ISO output location.  |

The state is populated before `ShouldProcess`, so it is available after `-WhatIf` and after a declined `-Confirm` operation. This global value is process state, not pipeline output.

## Output

`Update-OSDeployBootISO` does not intentionally write an object to the PowerShell pipeline. Output from the delegated ISO creation command is suppressed, including its `System.IO.FileInfo` result after successful creation.

Inspect `bootmedia.iso` and the optional `bootmedia_ca2023.iso` in `$global:BuildMedia.MediaRootPath` to verify completion. Errors from missing ADK tools, missing boot-sector files, or a nonzero `oscdimg.exe` exit code are written to the error stream.

See [Update a Boot ISO](../cmdlets/update-osdeploybootiso.md) for the workflow overview or the [Update-OSDeployBootISO command reference](../../command-reference/osdeploy/update-osdeploybootiso.md) for compact syntax and parameter definitions.

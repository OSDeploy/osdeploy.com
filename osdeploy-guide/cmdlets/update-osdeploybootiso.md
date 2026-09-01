---
description: >-
  Rebuild standard and CA 2023 ISO files from an existing OSDeploy Boot media
  build.
---

# Update-OSDeployBootISO

`Update-OSDeployBootISO` recreates bootable ISO files from the existing media trees in a selected OSDeploy Boot build. Use it after changing media files or saved OSDCloud profiles when `boot.wim` does not need to be serviced again.

The command rebuilds `bootmedia.iso` from `bootmedia`. When the selected build also contains `bootmedia_ca2023`, it rebuilds `bootmedia_ca2023.iso` from that tree. It does not mount, service, export, or replace `boot.wim`.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

The workstation must also have:

* The [OSDeploy module](../requirements/powershell-modules.md).
* The [Windows ADK Deployment Tools](/broken/pages/KKnKou096GC0HYAS6jiH), including `oscdimg.exe`.
* `Out-GridView` for selecting a completed build.
* A completed build under `C:\ProgramData\OSDeployCore\boot` with a `bootmedia` directory.

{% hint style="warning" %}
The function stops when a host requirement is not met, no build is selected, the Windows ADK is unavailable, or the selected build does not contain `bootmedia`. A build containing only `bootmedia_ca2023` appears in the selector but cannot be processed without the standard media tree.
{% endhint %}

## Parameters

The function has no command-specific parameters and does not accept pipeline input. Build selection is always interactive.

| Parameter  | Type             | Default     | Accepted values and behavior                                                                                                                                                           |
| ---------- | ---------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-WhatIf`  | Common parameter | Not enabled | Run requirement checks and build selection, verify ADK and media paths, populate `$global:BuildMedia`, and report the rebuild without invoking the ISO step.                           |
| `-Confirm` | Common parameter | Not enabled | Request confirmation before invoking the ISO step. The function declares `ConfirmImpact` as `Medium`, so it does not prompt under PowerShell's default `High` confirmation preference. |

## Examples

### Rebuild available ISO files

Select a completed build and recreate its standard ISO plus its CA 2023 ISO when that source tree exists:

```powershell
Update-OSDeployBootISO
```

### Request confirmation

Review the selected build path before allowing ISO output files to be replaced:

```powershell
Update-OSDeployBootISO -Confirm
```

The parent operation is the primary confirmation boundary. An inherited confirmation preference can also prompt in the delegated ISO creation operation.

### Preview the rebuild

Open the build selector, validate the ADK and media paths, and report the planned operation without copying profiles or starting `oscdimg.exe`:

```powershell
Update-OSDeployBootISO -WhatIf
```

### Inspect the rebuilt files

Use the global build state to inspect the ISO files after the command completes:

```powershell
Update-OSDeployBootISO

Get-ChildItem `
	-LiteralPath $global:BuildMedia.MediaRootPath `
	-Filter '*.iso' |
	Select-Object Name, Length, LastWriteTime
```

## Build Selection

The function searches immediate child directories under:

```
C:\ProgramData\OSDeployCore\boot
```

A directory is eligible when it contains either `bootmedia` or `bootmedia_ca2023`. Eligible builds are sorted by last-modified time, newest first, and displayed in a single-selection `Out-GridView` picker with `ModifiedTime`, `Name`, and `Path` properties.

Canceling the picker or having no eligible build writes a warning and returns. After selection, the command separately requires `bootmedia`; `bootmedia_ca2023` remains optional.

## Media and Architecture

The media paths and outputs are fixed:

| Source directory   | Output file            | Behavior                                                                                                                                             |
| ------------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `bootmedia`        | `bootmedia.iso`        | Required and always processed.                                                                                                                       |
| `bootmedia_ca2023` | `bootmedia_ca2023.iso` | Processed only when the directory exists. This tree contains the updated CA 2023 Secure Boot files produced from a compatible imported WinRE source. |

Both ISO files are written in the selected build root. Existing files with the same names can be replaced. The selected build directory name becomes the ISO volume label.

The command preserves the architecture of the selected media tree; it does not convert `amd64` media to `arm64` or the reverse. The ISO helper searches the installed ADK for `oscdimg.exe` under `amd64` first and `arm64` second. That tool-selection order does not determine the media architecture.

The resulting ISO is UEFI-only. The helper uses the first available `efisys_noprompt.bin` or `efisys.bin`, preferring the media tree before the ADK `Oscdimg` directory, and creates a UDF 1.02 image without a legacy BIOS boot entry.

## OSDCloud Profiles

Before creating each ISO, the shared ISO step checks:

```
C:\ProgramData\OSDeployCore\OSDCloud\Profiles
```

When that source exists, it copies the directory into `OSDCloud\Profiles` in the applicable media tree. It creates the ISO and then removes the destination profile directory. The same sequence runs independently for `bootmedia_ca2023` when present.

{% hint style="warning" %}
Cleanup removes the complete `OSDCloud\Profiles` directory from each processed media tree, including content that existed there before the command started. Keep persistent profiles in `C:\ProgramData\OSDeployCore\OSDCloud\Profiles`.
{% endhint %}

Profile copy and cleanup are not separately gated by `ShouldProcess`. The parent gate prevents the entire ISO step from running under `-WhatIf` or after a declined confirmation.

## ISO Creation and Failure Behavior

The helper requires `oscdimg.exe` and a UEFI boot-sector file. Missing tools or boot data write a non-terminating error and skip that ISO. A nonzero `oscdimg.exe` exit code also writes a non-terminating error; the command does not restore an earlier ISO that was replaced or partially written.

The helper enables file optimization and media larger than a standard CD. Its `System.IO.FileInfo` result is suppressed by the shared ISO step.

## WhatIf and Confirmation

The function performs host checks, initializes OSDeploy Core paths, opens the build selector, verifies the Windows ADK, resolves both media paths, populates `$global:BuildMedia`, and displays those paths before its single parent `ShouldProcess` call.

With `-WhatIf`, the function reports `Rebuild BootMedia ISO files` and does not call the ISO step. No profile directories are copied or removed, `oscdimg.exe` is not started, and existing ISO files are unchanged.

With `-Confirm`, declining the parent operation leaves the populated global state in place and returns without changing the media tree or ISO files.

## Output

`Update-OSDeployBootISO` does not intentionally write an object to the pipeline. Inspect `bootmedia.iso` and optional `bootmedia_ca2023.iso` in `$global:BuildMedia.MediaRootPath` to verify completion.

The command replaces `$global:BuildMedia` with an ordered dictionary containing `AdkRootPath`, `MediaIsoLabel`, `MediaPath`, `MediaPathEX`, and `MediaRootPath`. This process-wide state is available after `-WhatIf` or declined confirmation, but it is not pipeline output.

See [The OSDeploy Boot ISO](../inside-osdeploy/update-osdeploybootiso.md) for the related workflow or the [Update-OSDeployBootISO command reference](../../command-reference/osdeploy/update-osdeploybootiso.md) for compact syntax.

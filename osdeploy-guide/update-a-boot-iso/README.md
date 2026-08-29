---
description: Rebuild bootable ISO files from an existing OSDeploy Boot media tree.
---

# Update a Boot ISO

Use `Update-OSDeployBootISO` to select a completed OSDeploy Boot build and recreate its bootable ISO files from the existing media directories. The function rebuilds the standard ISO and, when available, the CA 2023 ISO without mounting or servicing `boot.wim`.

## Requirements

Run the function on a computer that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../module-setup/README.md)
* [Windows ADK Deployment Tools](../../core-components/microsoft-windows-adk/README.md)
* `Out-GridView` available for interactive build selection
* Administrator rights
* `curl.exe` available in `PATH`
* A completed OSDeploy Boot build with a `bootmedia` directory

{% hint style="warning" %}
The function stops without rebuilding an ISO when a host requirement is missing, no build is selected, the Windows ADK is unavailable, or the selected build does not contain `bootmedia`.
{% endhint %}

## Basic Usage

Open an elevated PowerShell 7.6 session and run the function without parameters:

```powershell
Update-OSDeployBootISO
```

Select one completed build in the `Out-GridView` window. The function uses these automatic settings:

| Setting | Default |
| --- | --- |
| Build location | Immediate child directory under `C:\ProgramData\OSDeployCore\boot` |
| Standard source | `bootmedia` |
| Standard output | `bootmedia.iso` |
| CA 2023 source | `bootmedia_ca2023`, when present |
| CA 2023 output | `bootmedia_ca2023.iso`, when present |
| ISO volume label | Selected build directory name |

{% hint style="info" %}
Use this workflow when files or saved OSDCloud profiles changed but `boot.wim` does not require servicing. Use [Build-OSDeployBoot](../build-boot-image/build-osdeployboot.md) when the boot image itself must be rebuilt.
{% endhint %}

## How It Works

`Update-OSDeployBootISO` performs the following actions:

1. Verifies Windows, PowerShell, `curl.exe`, and administrator requirements.
2. Searches `C:\ProgramData\OSDeployCore\boot` for completed builds and opens an interactive selector, sorted with the most recently modified build first.
3. Verifies the Windows ADK installation and requires the selected build to contain `bootmedia`.
4. Detects the optional `bootmedia_ca2023` directory and records the resolved media paths in `$global:BuildMedia`.
5. Copies saved profiles from `C:\ProgramData\OSDeployCore\OSDCloud\Profiles` into each applicable media tree when that profile source exists.
6. Uses the Windows ADK `oscdimg.exe` tool to replace `bootmedia.iso` and, when applicable, `bootmedia_ca2023.iso` in the selected build directory.
7. Removes the copied `OSDCloud\Profiles` directory from each media tree after the ISO creation attempt.

{% hint style="warning" %}
Profile cleanup removes the complete `OSDCloud\Profiles` directory from each processed media tree, including content that existed there before the command started. Keep persistent profiles in `C:\ProgramData\OSDeployCore\OSDCloud\Profiles`.
{% endhint %}

For confirmation, `WhatIf`, media selection, profile handling, and ISO creation details, see [Update-OSDeployBootISO](update-osdeploybootiso.md). For compact syntax and parameter definitions, see the [command reference](../../command-reference/osdeploy/update-osdeploybootiso.md).

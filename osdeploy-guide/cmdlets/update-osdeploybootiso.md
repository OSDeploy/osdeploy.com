---
description: Rebuild bootable ISO files from an existing OSDeploy Boot media tree.
---

# Update-OSDeployBootISO

Use `Update-OSDeployBootISO` to select a completed OSDeploy Boot build and recreate its bootable ISO files from the existing media directories. The function rebuilds the standard ISO and, when available, the CA 2023 ISO without mounting or servicing `boot.wim`.

{% hint style="info" %}
Use this workflow when files or saved OSDCloud profiles changed but `boot.wim` does not require servicing. Use [Build-OSDeployBoot](build-osdeployboot.md) when the boot image itself must be rebuilt.
{% endhint %}

## Rebuild the ISO Files

{% stepper %}
{% step %}
### Confirm the Requirements

Run the function on a computer that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../requirements/powershell-modules.md)
* [Windows ADK Deployment Tools](../../core-components/microsoft-windows-adk/)
* `Out-GridView` available for interactive build selection
* Administrator rights
* `curl.exe` available in `PATH`
* A completed OSDeploy Boot build with a `bootmedia` directory

{% hint style="warning" %}
The function stops without rebuilding an ISO when a host requirement is missing, no build is selected, the Windows ADK is unavailable, or the selected build does not contain `bootmedia`.
{% endhint %}
{% endstep %}

{% step %}
### Run the Update

Open an elevated PowerShell 7.6 session and run the function without parameters:

```powershell
Update-OSDeployBootISO
```

Select one completed build in the `Out-GridView` window. The function uses these automatic settings:

| Setting          | Default                                                            |
| ---------------- | ------------------------------------------------------------------ |
| Build location   | Immediate child directory under `C:\ProgramData\OSDeployCore\boot` |
| Standard source  | `bootmedia`                                                        |
| Standard output  | `bootmedia.iso`                                                    |
| CA 2023 source   | `bootmedia_ca2023`, when present                                   |
| CA 2023 output   | `bootmedia_ca2023.iso`, when present                               |
| ISO volume label | Selected build directory name                                      |

The function searches immediate child directories under `C:\ProgramData\OSDeployCore\boot`, sorts completed builds with the newest first, and opens a single-selection `Out-GridView` picker. It records the selected media paths in `$global:BuildMedia`, copies saved profiles into each applicable media tree, uses the Windows ADK `oscdimg.exe` tool to replace the ISO files, and then removes the copied profile directory.

{% hint style="warning" %}
Profile cleanup removes the complete `OSDCloud\Profiles` directory from each processed media tree, including content that existed there before the command started. Keep persistent profiles in `C:\ProgramData\OSDeployCore\OSDCloud\Profiles`.
{% endhint %}
{% endstep %}

{% step %}
### Verify the ISO Files

Inspect the ISO files in the selected build directory:

```powershell
Get-ChildItem `
	-LiteralPath $global:BuildMedia.MediaRootPath `
	-Filter '*.iso' |
	Select-Object Name, Length, LastWriteTime
```

Confirm that `bootmedia.iso` has a current modified time. When the selected build contains `bootmedia_ca2023`, confirm that `bootmedia_ca2023.iso` was also rebuilt. The function does not return the created files to the pipeline.
{% endstep %}
{% endstepper %}

For confirmation, `WhatIf`, media selection, profile handling, and ISO creation details, see [Update-OSDeployBootISO](../inside-osdeploy/update-osdeploybootiso.md). For compact syntax and parameter definitions, see the [command reference](../../command-reference/osdeploy/update-osdeploybootiso.md).

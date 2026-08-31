---
description: >-
  Prepare an OSDeploy workstation and create Hydra boot media with
  Invoke-OSDeployHydration.
---

# Quick Setup

Use `Invoke-OSDeployHydration` to prepare an OSDeploy workstation, download current Windows and WinPE driver content, and create OSDCloud boot media named `Hydra`.

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Hydration installs software, downloads several gigabytes of content, and creates boot media under `C:\ProgramData\OSDeployCore`. Enabling Hyper-V can require a restart, which Hydration does not perform.
{% endhint %}

## Run Hydration

{% stepper %}
{% step %}
### Confirm the Requirements

Use a workstation with:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](requirements/powershell-modules.md)
* OSDCloud module version `26.5.24.1` or later
* Administrator rights
* `curl.exe` available in `PATH`

Open PowerShell 7.6 as an administrator before continuing.
{% endstep %}

{% step %}
### Start Hydration

Run the interactive workflow:

```powershell
Invoke-OSDeployHydration
```

Review the workflow and confirm that Hydration can continue. The function detects the workstation architecture and processes the Windows content, drivers, and boot image for that architecture.
{% endstep %}

{% step %}
### Choose the Workstation Components

Approve installation of the Windows ADK 25H2 and 7-Zip when either component is missing. Both are required, and declining either installation stops Hydration.

Choose whether to install Git for Windows, Visual Studio Code, Visual Studio Code Insiders, and Hyper-V when prompted. These components are optional. Hyper-V installation and testing are offered only on a physical workstation.
{% endstep %}

{% step %}
### Complete the Boot Media Selections

Respond to any content or wallpaper selectors displayed by `Build-OSDeployBoot`. Hydration creates a boot image named `Hydra` from the newest imported WinRE source for the detected architecture. When no matching WinRE source is available, it uses the Windows ADK WinPE image.

On a physical workstation with Hyper-V enabled, choose whether to create a virtual machine and boot the generated ISO.
{% endstep %}

{% step %}
### Verify the Boot Media

Find the newest Hydra build and confirm that its ISO exists:

```powershell
$Hydra = Get-ChildItem -Path "$env:ProgramData\OSDeployCore\boot" -Directory |
	Where-Object Name -Like '*-Hydra*' |
	Sort-Object LastWriteTime -Descending |
	Select-Object -First 1

Get-Item -LiteralPath (Join-Path $Hydra.FullName 'bootmedia.iso')
```

The completed build directory contains `bootmedia`, `bootmedia.iso`, metadata, and logs.
{% endstep %}
{% endstepper %}

For `-Force`, `-WhatIf`, workflow details, and returned objects, see [Invoke-OSDeployHydration](advanced/invoke-osdeployhydration.md). For compact syntax and parameter definitions, see the [Invoke-OSDeployHydration command reference](../command-reference/osdeploy/invoke-osdeployhydration.md).

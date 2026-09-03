---
description: >-
  Prepare an OSDeploy workstation and create Hydra boot media with
  Invoke-OSDeployHydration.
---

# Quick Setup

Use `Invoke-OSDeployHydration` to prepare an OSDeploy workstation, download current Windows and WinPE driver content, and create OSDCloud boot media named `Hydra`.

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Hydration installs software, downloads Windows and driver content, and creates boot media under `C:\ProgramData\OSDeployCore`. If Hydration enables Hyper-V, restart Windows before creating a test VM.
{% endhint %}

## Run Hydration

{% stepper %}
{% step %}
### Confirm the Requirements

Use a workstation with:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../osdeploy-guide/requirements/powershell-modules.md)
* OSDCloud module version `26.5.24.1` or later
* A valid Recast Software Community License, which is required while the OSDeploy module is in preview; complete [Community Registration](registration.md) to install it
* Administrator rights
* `curl.exe` available in `PATH`

Open PowerShell 7.6 as an administrator before continuing.
{% endstep %}

{% step %}
### Run Hydration

Run the interactive workflow from the elevated PowerShell 7.6 session:

```powershell
Invoke-OSDeployHydration
```

Review the workflow and continue. Approve the Windows ADK 25H2 and 7-Zip installations when prompted; declining either required component stops Hydration. Choose whether to install the optional Git, Visual Studio Code, Visual Studio Code Insiders, and Hyper-V components.

Complete any content and wallpaper selections. Hydration detects the workstation architecture, prepares matching Windows and WinPE driver content, and creates Hydra boot media under `C:\ProgramData\OSDeployCore\boot`. On a physical workstation with Hyper-V enabled, it can also create a VM and boot the generated ISO.
{% endstep %}
{% endstepper %}

For `-Force`, `-WhatIf`, workflow details, and returned objects, see [Invoke-OSDeployHydration](cmdlets/invoke-osdeployhydration.md). For compact syntax and parameter definitions, see the [Invoke-OSDeployHydration command reference](../command-reference/osdeploy/invoke-osdeployhydration.md).

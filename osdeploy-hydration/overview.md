---
description: >-
  Understand how Invoke-OSDeployHydration prepares an OSDeploy Core Workstation
  and creates OSDCloud BootMedia.
---

# Overview

OSDeploy Hydration is the end-to-end workstation preparation workflow provided by the `Invoke-OSDeployHydration` function. It installs the required build tools, downloads the current Windows and driver content, and creates OSDCloud BootMedia in a single coordinated process.

Use Hydration to prepare a new OSDeploy Core Workstation or refresh the core content needed to build a current boot image. The function detects the workstation architecture and runs each stage in the required order.

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Hydration prepares the workstation and creates BootMedia. OSDCloud runs from that media on the target device and performs the Windows deployment.
{% endhint %}

## Hydration Process

`Invoke-OSDeployHydration` performs the following major steps:

1. **Validate the workstation.** The function verifies the supported Windows version, PowerShell installation, administrator access, required commands, and the OSDCloud module.
2. **Prepare the build tools.** Hydration installs the Windows ADK and 7-Zip when required. It can also install Git for Windows, Visual Studio Code, Visual Studio Code Insiders, and Hyper-V as optional workstation tools.
3. **Download Windows content.** The function downloads the latest supported Windows Enterprise ESD for the detected AMD64 or ARM64 architecture.
4. **Import the operating system.** OSDeploy converts the downloaded ESD into the Windows operating system and WinRE sources used by later build operations.
5. **Update WinPE drivers.** Hydration downloads and expands current vendor and Microsoft network, storage, and wireless driver content for the detected architecture.
6. **Build BootMedia.** The function runs `Build-OSDeployBoot` in automatic mode and creates a boot image named `Hydra`. It uses the newest available WinRE source and falls back to the Windows ADK WinPE image when no WinRE source is available.
7. **Optionally test the media.** On a physical workstation with Hyper-V enabled, Hydration can create a virtual machine and boot the generated ISO to validate the completed BootMedia.

## Interactive and Automatic Operation

By default, Hydration displays the planned workflow and prompts before installing software or performing optional actions. Required software must be installed before the workflow can continue.

Use the `-Force` parameter to accept the required installation and workflow choices and run the hydration process non-interactively. Optional Hyper-V testing is also performed when the workstation supports it and the generated ISO is available.

# Core Components

Core Components are the software tools and Windows features that must be in place on a Windows 11 build machine before running any OSDeploy workflow.

## Overview

OSDeploy runs on a Windows 11 workstation or server (amd64 or arm64) and uses these components at build time — not inside WinPE at deployment time. Some components are required for every workflow; others are needed only for specific tasks such as MDT integration, virtual machine testing, or script development. Use `Install-OSDeploySoftware` to install and manage all supported components from a single PowerShell command.

| Component | Purpose | Required | Install Method |
|---|---|---|---|
| PowerShell 7.6+ | Required for all OSDeploy, OSDCloud, and OSD module commands | Required | MSI installer |
| OSDeploy module | WinPE boot image creation on Windows 11 | Required | `Install-Module` |
| OSDCloud module | Windows 11 deployment from WinPE | Required for OSDCloud | `Install-Module` |
| OSD module | Legacy OSDCloud v1 and WinPE deployment | Optional (legacy) | `Install-Module` |
| Windows ADK + WinPE add-on | Tooling to create and customize WinPE boot images | Required | `Install-OSDeploySoftware` |
| Microsoft Deployment Toolkit | Legacy MDT integration workflows | Optional | `Install-OSDeploySoftware` |
| Hyper-V | Local VM creation for testing boot media | Optional | `Install-OSDeploySoftware` |
| Git for Windows | Source control for scripts and configurations | Optional | `Install-OSDeploySoftware` |
| Visual Studio Code | Script authoring and editing | Optional | `Install-OSDeploySoftware` |

{% hint style="info" %}
All components except PowerShell 7 and the PowerShell modules can be installed or downloaded using a single function: `Install-OSDeploySoftware`. Run it without parameters to list all available components and the command to install each one.
{% endhint %}

## In This Section

- [PowerShell](powershell/README.md) — Install PowerShell 7.6 or later (MSI package)
- [PowerShell Modules](powershell-modules/README.md) — Install the OSDeploy, OSDCloud, and OSD modules from the PowerShell Gallery
- [Microsoft Windows ADK](microsoft-windows-adk/README.md) — Install the Windows Assessment and Deployment Kit and WinPE add-on
- [Microsoft Deployment Toolkit](microsoft-deployment-toolkit/README.md) — Install MDT for existing MDT-based deployment workflows (retired)
- [WinPE Drivers](winpe-drivers/README.md) — Download vendor driver packs for injection into WinPE boot images
- [Windows Components](windows-components/README.md) — Enable optional Windows features such as Hyper-V
- [Developer Tools](developer-tools/README.md) — Optional tools for authoring and customizing OSDeploy workflows

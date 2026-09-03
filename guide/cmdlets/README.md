---
description: Use task-oriented guides for the public commands in the OSDeploy PowerShell module.
---

# Cmdlets

Use these guides to run OSDeploy commands and understand their requirements, prompts, selection behavior, side effects, and output. Start with [Basic Setup](../basic/README.md) for the shortest workstation setup workflow. Use the [OSDeploy command reference](../../command-reference/osdeploy/README.md) for compact syntax and output lookup.

{% hint style="warning" %}
A valid Recast Software Community License is required while the OSDeploy module is in preview. Complete [Community Registration](../registration.md) before running OSDeploy commands.
{% endhint %}

## Workstation Setup

| Guide | Use it to |
| --- | --- |
| [Get-OSDeployModulePath](get-osdeploymodulepath.md) | Locate resources in the OSDeploy module that is loaded in the current session. |
| [Get-OSDeployModuleVersion](get-osdeploymoduleversion.md) | Return or compare the loaded OSDeploy module version. |
| [Show-OSDeployLicense](show-osdeploylicense.md) | Inspect the selected Recast Software license candidate or display registration guidance. |
| [Invoke-OSDeployHydration](invoke-osdeployhydration.md) | Prepare a workstation, refresh Windows and driver content, build boot media, and optionally test it in Hyper-V. |
| [Install-OSDeploySoftware](install-osdeploysoftware/README.md) | Install individual OSDeploy workstation prerequisites and tools. |

## Core Content and Boot Media

| Guide | Use it to |
| --- | --- |
| [Update-OSDeployCore](update-osdeploycore.md) | Coordinate the Windows ESD, imported Windows image, and WinPE driver updates. |
| [Update-OSDeployCoreESD](update-osdeploycoreesd.md) | Download and verify Windows Enterprise ESD content. |
| [Update-OSDeployCoreOS](update-osdeploycoreos.md) | Import Windows OS and WinRE images from cached ESD files. |
| [Update-OSDeployCoreDrivers](update-osdeploycoredrivers.md) | Download and expand WinPE driver packages. |
| [Build-OSDeployBoot](build-osdeployboot.md) | Build customized WinPE media from imported WinRE or Windows ADK WinPE. |
| [Update-OSDeployBootISO](update-osdeploybootiso.md) | Rebuild ISO files for an existing OSDeploy boot-media build. |

## USB, Hyper-V, and MDT

| Guide | Use it to |
| --- | --- |
| [New-OSDeployBootUSB](new-osdeploybootusb.md) | Prepare a bootable USB disk from completed OSDeploy media. |
| [Update-OSDeployBootUSB](update-osdeploybootusb.md) | Refresh an existing OSDeploy USB volume from completed media. |
| [New-OSDeployHyperVM](new-osdeployhypervm.md) | Create and start a Hyper-V test VM from an OSDeploy ISO. |
| [Install-OSDeployMDT](install-osdeploymdt.md) | Add OSDeploy integration to an MDT deployment share. |
| [Invoke-OSDeployMDT](invoke-osdeploymdt.md) | Run the OSDeploy MDT integration stages. |

{% hint style="warning" %}
Several commands install software, download large files, service Windows images, modify USB disks, or create virtual machines. Review each guide's requirements, confirmation behavior, and side effects before running it in production.
{% endhint %}

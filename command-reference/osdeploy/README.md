# OSDeploy Module

The **OSDeploy** module is used on **Windows 11 25H2** to create WinPE boot images. It runs in a full Windows OS environment — not in WinPE.

| Property     | Value                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------- |
| Publisher    | Community / David Segura                                                                        |
| Gallery      | [powershellgallery.com/packages/OSDeploy](https://www.powershellgallery.com/packages/OSDeploy/) |
| Platform     | Windows 11 25H2                                                                                 |
| Architecture | amd64 / arm64                                                                                   |
| Status       | Preview                                                                                         |
| Required by  | OSDeploy deployment workflows                                                                   |

{% embed url="https://www.powershellgallery.com/packages/OSDeploy/" %}

## Overview

The OSDeploy module is the build-time counterpart to OSDCloud. It runs on a full **Windows 11 25H2 or later** installation — not inside WinPE — and is responsible for creating and customizing WinPE boot images. The module automates the entire boot image pipeline: pulling in Windows ADK optional components and language packs, injecting WinPE drivers, embedding the OSDCloud PowerShell module, adding WinPE apps and console configuration, and generating bootable ISO files and USB drives.

{% hint style="warning" %}
A valid Recast Software Community License is required while the OSDeploy module is in preview. Complete [Community Registration](../../guide/registration.md) before running OSDeploy commands. This requirement does not apply to standalone use of the OSDCloud or legacy OSD modules.
{% endhint %}

Use `Invoke-OSDeployHydration` for a fully automated end-to-end build, or use `Build-OSDeployBoot` directly for fine-grained control over individual boot image builds.

## Install

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
```

***

## Related

* [OSDeploy on PowerShell Gallery](https://www.powershellgallery.com/packages/OSDeploy/)

***

## Functions

The OSDeploy module (version 26.5.1.1) exports 17 public functions across seven functional areas.

### Module Utilities

| Function                                                  | Description                                                        |
| --------------------------------------------------------- | ------------------------------------------------------------------ |
| [Get-OSDeployModulePath](get-osdeploymodulepath.md)       | Returns the file system path to the OSDeploy module root directory |
| [Get-OSDeployModuleVersion](get-osdeploymoduleversion.md) | Returns the currently loaded OSDeploy module version               |

### OSDeployCore

| Function                                            | Description                                                                      |
| --------------------------------------------------- | -------------------------------------------------------------------------------- |
| [Update-OSDeployCore](update-osdeploycore.md)       | Updates all OSDeployCore assets: ESD files, OS images, and WinPE driver packages |
| [Update-OSDeployCoreESD](update-osdeploycoreesd.md) | Downloads Windows Enterprise ESD files from the latest OSDeploy OS catalog       |
| [Update-OSDeployCoreOS](update-osdeploycoreos.md)   | Imports Windows OS images from cached Enterprise ESD files to OSDeployCore       |

### BootMedia

| Function                                                | Description                                                               |
| ------------------------------------------------------- | ------------------------------------------------------------------------- |
| [Build-OSDeployBoot](build-osdeployboot.md)             | Builds a customized WinPE boot image from a WinRE or ADK WinPE source     |
| [Invoke-OSDeployHydration](invoke-osdeployhydration.md) | Runs the full OSDeploy hydration workflow end-to-end                      |
| [Update-OSDeployBootISO](update-osdeploybootiso.md)     | Rebuilds bootable ISO files for an existing BootImage build               |
| [Import-OSDeployCoreOS](import-osdeploycoreos.md)       | Imports Windows OS images from mounted installation media to OSDeployCore |

### MDT Integration

| Function                                      | Description                                                         |
| --------------------------------------------- | ------------------------------------------------------------------- |
| [Install-OSDeployMDT](install-osdeploymdt.md) | Initializes an MDT Deployment Share for OSDeploy                    |
| [Invoke-OSDeployMDT](invoke-osdeploymdt.md)   | MDT LiteTouchPE exit script — runs on every Update Deployment Share |

### Software Installation

| Function                                                | Description                                             |
| ------------------------------------------------------- | ------------------------------------------------------- |
| [Install-OSDeploySoftware](install-osdeploysoftware.md) | Installs OS deployment prerequisite software on Windows |

### USB

| Function                                            | Description                                               |
| --------------------------------------------------- | --------------------------------------------------------- |
| [New-OSDeployBootUSB](new-osdeploybootusb.md)       | Creates a new bootable OSDeploy USB drive                 |
| [Update-OSDeployBootUSB](update-osdeploybootusb.md) | Updates an existing OSDeploy USB drive with new BootMedia |

### Virtual Machines

| Function                                      | Description                                              |
| --------------------------------------------- | -------------------------------------------------------- |
| [New-OSDeployHyperVM](new-osdeployhypervm.md) | Creates a Hyper-V VM pre-configured for OSDeploy testing |

### WinPE Drivers

| Function                                                    | Description                                                   |
| ----------------------------------------------------------- | ------------------------------------------------------------- |
| [Get-OSDeployCoreDrivers](get-osdeploycoredrivers.md)       | Returns WinPE driver folders from the OSDeployCore library    |
| [Update-OSDeployCoreDrivers](update-osdeploycoredrivers.md) | Downloads and expands WinPE driver packages into OSDeployCore |

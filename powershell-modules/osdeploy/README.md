# OSDeploy PowerShell Module

The **OSDeploy** module is used on **Windows 11 25H2** to create WinPE boot images. It runs in a full Windows OS environment — not in WinPE.

| Property     | Value                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------- |
| Publisher    | Community / David Segura                                                                        |
| Gallery      | [powershellgallery.com/packages/OSDeploy](https://www.powershellgallery.com/packages/OSDeploy/) |
| Platform     | Windows 11 25H2                                                                                 |
| Architecture | amd64 / arm64                                                                                   |
| Status       | Current / Recommended                                                                           |
| Required by  | OSDeploy deployment workflows                                                                   |

{% embed url="https://www.powershellgallery.com/packages/OSDeploy/" %}

## Overview

The OSDeploy module is the build-time counterpart to OSDCloud. It runs on a full **Windows 11 25H2 or later** installation — not inside WinPE — and is responsible for creating and customizing WinPE boot images. The module automates the entire boot image pipeline: pulling in Windows ADK optional components and language packs, injecting WinPE drivers, embedding the OSDCloud PowerShell module, adding WinPE apps and console configuration, and generating bootable ISO files and USB drives.

Use `Invoke-OSDeployHydration` for a fully automated end-to-end build, or use `Build-OSDeployBootMedia` directly for fine-grained control over individual boot image builds.

## Install

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
```

***

## Related

* [OSDeploy on PowerShell Gallery](https://www.powershellgallery.com/packages/OSDeploy/)

***

## Functions

The OSDeploy module (version 26.5.1.1) exports 14 public functions across six functional areas.

### Module Utilities

| Function | Description |
|---|---|
| [Get-OSDeployModulePath](Get-OSDeployModulePath.md) | Returns the file system path to the OSDeploy module root directory |
| [Get-OSDeployModuleVersion](Get-OSDeployModuleVersion.md) | Returns the currently loaded OSDeploy module version |

### BootMedia

| Function | Description |
|---|---|
| [Build-OSDeployBootMedia](Build-OSDeployBootMedia.md) | Builds a customized WinPE boot image from a WinRE or ADK WinPE source |
| [Invoke-OSDeployHydration](Invoke-OSDeployHydration.md) | Runs the full OSDeploy hydration workflow end-to-end |
| [Update-OSDeployISO](Update-OSDeployISO.md) | Rebuilds bootable ISO files for an existing BootImage build |
| [Import-OSDeployOS](Import-OSDeployOS.md) | Imports Windows OS images from mounted installation media to OSDeployCore |

### MDT Integration

| Function | Description |
|---|---|
| [Install-OSDeployMDT](Install-OSDeployMDT.md) | Initializes an MDT Deployment Share for OSDeploy |
| [Invoke-OSDeployMDT](Invoke-OSDeployMDT.md) | MDT LiteTouchPE exit script — runs on every Update Deployment Share |

### Software Installation

| Function | Description |
|---|---|
| [Install-OSDeploySoftware](Install-OSDeploySoftware.md) | Installs OS deployment prerequisite software on Windows |

### USB

| Function | Description |
|---|---|
| [New-OSDeployUSB](New-OSDeployUSB.md) | Creates a new bootable OSDeploy USB drive |
| [Update-OSDeployUSB](Update-OSDeployUSB.md) | Updates an existing OSDeploy USB drive with new BootMedia |

### Virtual Machines

| Function | Description |
|---|---|
| [New-OSDeployHyperVM](New-OSDeployHyperVM.md) | Creates a Hyper-V VM pre-configured for OSDeploy testing |

### WinPE Drivers

| Function | Description |
|---|---|
| [Get-OSDeployWinPEDrivers](Get-OSDeployWinPEDrivers.md) | Returns WinPE driver folders from the OSDeployCore library |
| [Update-OSDeployWinPEDrivers](Update-OSDeployWinPEDrivers.md) | Downloads and expands WinPE driver packages into OSDeployCore |

# Functions

The OSDeploy module (version 26.5.1.1) exports 14 public functions across six functional areas.

## Module Utilities

| Function | Description |
|---|---|
| [Get-OSDeployModulePath](Get-OSDeployModulePath.md) | Returns the file system path to the OSDeploy module root directory |
| [Get-OSDeployModuleVersion](Get-OSDeployModuleVersion.md) | Returns the currently loaded OSDeploy module version |

## Boot Image

| Function | Description |
|---|---|
| [Build-OSDeployBootMedia](Build-OSDeployBootMedia.md) | Builds a customized WinPE boot image from a WinRE or ADK WinPE source |
| [Invoke-OSDeployHydration](Invoke-OSDeployHydration.md) | Runs the full OSDeploy hydration workflow end-to-end |
| [Update-OSDeployISO](Update-OSDeployISO.md) | Rebuilds bootable ISO files for an existing BootImage build |

## USB

| Function | Description |
|---|---|
| [New-OSDeployUSB](New-OSDeployUSB.md) | Creates a new bootable OSDeploy USB drive |
| [Update-OSDeployUSB](Update-OSDeployUSB.md) | Updates an existing OSDeploy USB drive with new BootMedia |

## MDT Integration

| Function | Description |
|---|---|
| [Install-OSDeployMDT](Install-OSDeployMDT.md) | Initializes an MDT Deployment Share for OSDeploy |
| [Invoke-OSDeployMDT](Invoke-OSDeployMDT.md) | MDT LiteTouchPE exit script — runs on every Update Deployment Share |

## Software and Setup

| Function | Description |
|---|---|
| [Install-OSDeploySoftware](Install-OSDeploySoftware.md) | Installs OS deployment prerequisite software on Windows |
| [Import-OSDeployOS](Import-OSDeployOS.md) | Imports Windows OS images from mounted installation media to OSDeployCore |

## WinPE Drivers

| Function | Description |
|---|---|
| [Get-OSDeployWinPEDrivers](Get-OSDeployWinPEDrivers.md) | Returns WinPE driver folders from the OSDeployCore library |
| [Update-OSDeployWinPEDrivers](Update-OSDeployWinPEDrivers.md) | Downloads and expands WinPE driver packages into OSDeployCore |

## Hyper-V

| Function | Description |
|---|---|
| [New-OSDeployHyperVM](New-OSDeployHyperVM.md) | Creates a Hyper-V VM pre-configured for OSDeploy testing |

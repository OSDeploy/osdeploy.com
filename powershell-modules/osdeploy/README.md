# PowerShell Modules

The following PowerShell modules are published to the [PowerShell Gallery](https://www.powershellgallery.com/) and are required for OSDeploy workflows. Install them before proceeding with any OSDeploy, OSDCloud, or MDT-based tasks.

Always use `-SkipPublisherCheck` when installing these modules. `-Force` allows reinstall or upgrade without a confirmation prompt.

***

## OSDeploy

The **OSDeploy** module is used on **Windows 11 25H2** to create WinPE boot images. It runs in a full Windows OS environment — not in WinPE.

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
```

See the [Functions](functions.md) page for the full list of available commands, or navigate directly to any function reference page in the sidebar.

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

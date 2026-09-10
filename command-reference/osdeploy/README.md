# OSDeploy Module

The **OSDeploy** module runs on Windows 11 25H2 to create and maintain WinPE boot media. It runs in a full Windows environment, not in WinPE.

| Property | Value |
| --- | --- |
| Module version | 26.9.10 |
| Gallery | [powershellgallery.com/packages/OSDeploy](https://www.powershellgallery.com/packages/OSDeploy/) |
| Platform | Windows 11 25H2 |
| Architecture | amd64 / arm64 |
| Status | Preview |

{% hint style="warning" %}
A valid Recast Software Community License is required for gated OSDeploy commands while the module is in preview. Complete [Community Registration](../../guide/registration.md) before running the build and update workflows.
{% endhint %}

## Install

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
```

## Functions

OSDeploy 26.9.10 exports 20 public functions.

### Module and License

| Function | Description |
| --- | --- |
| [Get-OSDeployModulePath](get-osdeploymodulepath.md) | Return the loaded OSDeploy module path. |
| [Get-OSDeployModuleVersion](get-osdeploymoduleversion.md) | Return the loaded OSDeploy module version. |
| [Import-OSDeployLicense](import-osdeploylicense.md) | Import and reconcile a Recast Software license. |
| [Show-OSDeployLicense](show-osdeploylicense.md) | Return the selected valid license or registration guidance. |

### Hydration and Software

| Function | Description |
| --- | --- |
| [Invoke-OSDeployHydration](invoke-osdeployhydration.md) | Run the workstation hydration workflow. |
| [Install-OSDeploySoftware](install-osdeploysoftware.md) | Inspect, download, or install deployment prerequisites. |

### OSDeploy Core

| Function | Description |
| --- | --- |
| [Update-OSDeployCore](update-osdeploycore.md) | Run the ESD, recovery, and driver update stages. |
| [Update-OSDeployCoreESD](update-osdeploycoreesd.md) | Download and verify Enterprise ESD files. |
| [Update-OSDeployCoreRE](update-osdeploycorere.md) | Export WinRE and stage supporting Windows OS content. |
| [Update-OSDeployCoreDrivers](update-osdeploycoredrivers.md) | Download and expand WinPE driver packages. |

### Boot Media and Profiles

| Function | Description |
| --- | --- |
| [Build-OSDeployBoot](build-osdeployboot.md) | Build customized WinPE media from WinRE or ADK WinPE. |
| [New-OSDeployBootProfilePreview](new-osdeploybootprofilepreview.md) | Create a persistent boot profile. |
| [Update-OSDeployBootProfilePreview](update-osdeploybootprofilepreview.md) | Update a persistent boot profile. |
| [Delete-OSDeployBootProfilePreview](delete-osdeploybootprofilepreview.md) | Delete a persistent boot profile. |
| [Update-OSDeployBootISO](update-osdeploybootiso.md) | Rebuild ISO files for completed boot media. |
| [New-OSDeployBootUSB](new-osdeploybootusb.md) | Prepare a new bootable USB disk. |
| [Update-OSDeployBootUSB](update-osdeploybootusb.md) | Refresh an existing boot USB. |
| [New-OSDeployHyperVM](new-osdeployhypervm.md) | Create a Hyper-V test VM from an OSDeploy ISO. |

### MDT Integration

| Function | Description |
| --- | --- |
| [Install-OSDeployMDT](install-osdeploymdt.md) | Initialize an MDT deployment share for OSDeploy. |
| [Invoke-OSDeployMDT](invoke-osdeploymdt.md) | Run the OSDeploy MDT integration stages. |

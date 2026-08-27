# OSDCloud Module

The OSDCloud module is the current, recommended PowerShell module for deploying Windows 11 from WinPE using cloud-based OS images and vendor driver packs.

| Property     | Value                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------- |
| Publisher    | Community / David Segura                                                                        |
| Gallery      | [powershellgallery.com/packages/OSDCloud](https://www.powershellgallery.com/packages/OSDCloud/) |
| Platform     | WinPE (Windows Preinstallation Environment)                                                     |
| Architecture | amd64 / arm64                                                                                   |
| Status       | Current / Recommended                                                                           |
| Required by  | OSDCloud deployment workflows                                                                   |

{% embed url="https://www.powershellgallery.com/packages/OSDCloud/" %}

## Overview

The OSDCloud module runs **inside WinPE** and handles the full cloud-based OS deployment workflow: it downloads the Windows 11 image from Microsoft, downloads OEM driver packs from vendor CDNs (HP, Dell, Lenovo, and others), applies the OS, and configures the device for first boot. It does not require a deployment server — all content is downloaded at runtime over the network.

The OSDCloud module supersedes the OSDCloud v1 implementation that existed in the OSD module. It is the preferred module for all new OSDCloud deployments.

## Why OSDCloud Is Preferred Over OSD for OSDCloud Workflows

* Actively maintained with first-class support for current Windows 11 releases
* Dedicated module with a focused scope — not bundled with unrelated functions
* OSDCloud v1 in the OSD module is legacy; the OSDCloud module receives feature development
* Supports both amd64 and arm64 WinPE environments

{% hint style="info" %}
The OSDCloud module is used **in WinPE** during deployment, not on the Windows 11 build machine. Install it into the WinPE image or run `Install-Module` from within a WinPE session.
{% endhint %}

## Install

```powershell
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

***

## Related

* [OSDCloud on PowerShell Gallery](https://www.powershellgallery.com/packages/OSDCloud/)
* [OSD Module](https://github.com/OSDeploy/osdeploy.com/blob/main/powershell-modules/osdcloud/osd-module.md) — Legacy module; OSDCloud v1
* [OSDeploy Module](https://github.com/OSDeploy/osdeploy.com/blob/main/powershell-modules/osdcloud/osdeploy/README.md) — Boot image creation on Windows 11

***

## Functions

The OSDCloud module exports public functions across two environments: the full Windows OS and WinPE.

### Module Utilities

| Function                                                  | Description                                                        |
| --------------------------------------------------------- | ------------------------------------------------------------------ |
| [Get-OSDCloudModulePath](get-osdcloudmodulepath.md)       | Returns the file system path to the OSDCloud module root directory |
| [Get-OSDCloudModuleVersion](get-osdcloudmoduleversion.md) | Returns the currently loaded OSDCloud module version               |

### Deployment

| Function                              | Description                                                                                       |
| ------------------------------------- | ------------------------------------------------------------------------------------------------- |
| [Deploy-OSDCloud](deploy-osdcloud.md) | Starts an OSDCloud OS deployment — launches the graphical UX or runs the CLI workflow immediately |

### Device Information

| Function                                              | Description                                                                               |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [Show-OSDCloudDeviceInfo](show-osdclouddeviceinfo.md) | Displays comprehensive WinPE device and hardware information during OS deployment startup |

### Tools

| Function                                            | Description                                                     |
| --------------------------------------------------- | --------------------------------------------------------------- |
| [Start-OSDCloudExplorer](start-osdcloudexplorer.md) | Opens a graphical file browser for WinPE and WinRE environments |

### WinPE Startup

These functions run only inside WinPE (`SystemDrive == X:`).

| Function                                                          | Description                                                                  |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| [Invoke-WinPEStartup](invoke-winpestartup.md)                     | Runs the full OSDCloud WinPE startup workflow from a single entry point      |
| [Invoke-WinPEStartupManager](invoke-winpestartupmanager.md)       | Invokes a WinPE startup utility action by Id                                 |
| [Show-WinPEStartupDeviceErrors](show-winpestartupdeviceerrors.md) | Displays WinPE Plug and Play devices with non-OK status                      |
| [Show-WinPEStartupDevices](show-winpestartupdevices.md)           | Displays the full WinPE Plug and Play device inventory                       |
| [Show-WinPEStartupIpconfig](show-winpestartupipconfig.md)         | Displays IP configuration details in WinPE                                   |
| [Show-WinPEStartupWifi](show-winpestartupwifi.md)                 | Establishes and validates Wi-Fi connectivity in WinPE                        |
| [Update-WinPEStartupModule](update-winpestartupmodule.md)         | Installs or updates a PowerShell module from the PowerShell Gallery in WinPE |

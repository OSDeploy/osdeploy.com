# PowerShell Modules

Three PowerShell modules support OSDeploy workflows. Each targets a different execution context — a full Windows OS build machine, WinPE, or both.

| Property | Value |
|---|---|
| Publisher | Community / David Segura |
| Registry | [PowerShell Gallery](https://www.powershellgallery.com/) |
| Architecture | amd64 / arm64 |
| Install flag | Always use `-Force -SkipPublisherCheck` |

## Overview

The **OSDeploy** module runs on a full Windows 11 OS to create WinPE boot images. The **OSDCloud** module runs inside WinPE to deploy Windows 11 to a device from the cloud. The **OSD** module also runs in WinPE and supports the older OSDCloud v1 implementation. For new deployments, the OSDCloud module is preferred over OSD for all OSDCloud functionality.

## Why OSDeploy Requires These Modules

- `OSDeploy` provides `Build-OSDeployBootMedia`, `Update-OSDeployBootMedia`, and all boot image build commands
- `OSDCloud` provides the current, recommended OSDCloud deployment commands used inside WinPE
- `OSD` is maintained for existing environments that depend on OSDCloud v1; not required for new workflows

{% hint style="info" %}
For new OSDCloud deployments, use the **OSDCloud** module. The OSDCloud functionality in the OSD module (v1) is legacy and the OSDCloud module is preferred for all new work.
{% endhint %}

## In This Section

- [OSDeploy Module](psmodules.md) — Install the OSDeploy, OSDCloud, and OSD modules
- [OSDCloud Module](osdcloud-module.md) — Current, recommended module for OSDCloud deployments in WinPE
- [OSD Module](osd-module.md) — Legacy module; maintains OSDCloud v1 and WinPE deployment support

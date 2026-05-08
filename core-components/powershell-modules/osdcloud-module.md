# OSDCloud Module

The OSDCloud module is the current, recommended PowerShell module for deploying Windows 11 from WinPE using cloud-based OS images and vendor driver packs.

| Property | Value |
|---|---|
| Publisher | Community / David Segura |
| Gallery | [powershellgallery.com/packages/OSDCloud](https://www.powershellgallery.com/packages/OSDCloud/) |
| Platform | WinPE (Windows Preinstallation Environment) |
| Architecture | amd64 / arm64 |
| Status | Current / Recommended |
| Required by | OSDCloud deployment workflows |

{% embed url="https://www.powershellgallery.com/packages/OSDCloud/" %}
{% endembed %}

## Overview

The OSDCloud module runs **inside WinPE** and handles the full cloud-based OS deployment workflow: it downloads the Windows 11 image from Microsoft, downloads OEM driver packs from vendor CDNs (HP, Dell, Lenovo, and others), applies the OS, and configures the device for first boot. It does not require a deployment server — all content is downloaded at runtime over the network.

The OSDCloud module supersedes the OSDCloud v1 implementation that existed in the OSD module. It is the preferred module for all new OSDCloud deployments.

## Why OSDCloud Is Preferred Over OSD for OSDCloud Workflows

- Actively maintained with first-class support for current Windows 11 releases
- Dedicated module with a focused scope — not bundled with unrelated functions
- OSDCloud v1 in the OSD module is legacy; the OSDCloud module receives feature development
- Supports both amd64 and arm64 WinPE environments

{% hint style="info" %}
The OSDCloud module is used **in WinPE** during deployment, not on the Windows 11 build machine. Install it into the WinPE image or run `Install-Module` from within a WinPE session.
{% endhint %}

## Install

```powershell
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

---

## Related

- [OSDCloud on PowerShell Gallery](https://www.powershellgallery.com/packages/OSDCloud/)
- [OSD Module](osd-module.md) — Legacy module; OSDCloud v1
- [OSDeploy Module](psmodules.md) — Boot image creation on Windows 11

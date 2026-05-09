# OSD Module

The OSD module is a maintained PowerShell module that provides WinPE-based Windows deployment and is the original home of OSDCloud (v1).

{% hint style="warning" %}
For OSDCloud deployments, use the **OSDCloud** module instead of OSD. The OSDCloud module supersedes the OSDCloud v1 implementation in OSD. The OSD module is maintained but the OSDCloud module is preferred for all new work.
{% endhint %}

| Property | Value |
|---|---|
| Publisher | Community / David Segura |
| Gallery | [powershellgallery.com/packages/OSD](https://www.powershellgallery.com/packages/OSD/) |
| Platform | WinPE |
| Architecture | amd64 / arm64 |
| Status | Maintained (OSDCloud v1 / legacy) |
| Required by | Existing OSDCloud v1 workflows |

{% embed url="https://www.powershellgallery.com/packages/OSD/" %}
{% endembed %}

## Overview

The OSD module was the original host of OSDCloud. The OSDCloud functionality was later extracted into the dedicated **OSDCloud** module, which is the current, recommended choice for all new deployments. The OSD module continues to be maintained and is required for environments that have existing OSDCloud v1 workflows or scripts built against it. For any new deployment project, start with the OSDCloud module.

## When to Use the OSD Module

- Your existing scripts use `Start-OSDCloud` or other OSD-based OSDCloud v1 commands
- Your WinPE image was built with OSD-based tooling and you have not yet migrated
- You have a specific OSD function dependency outside of OSDCloud v1

## Install

```powershell
Install-Module -Name OSD -Force -SkipPublisherCheck
```

---

## Related

- [OSD on PowerShell Gallery](https://www.powershellgallery.com/packages/OSD/)
- [OSDCloud Module](osdcloud-module.md) — Current, recommended module for OSDCloud deployments
- [OSDeploy Module](psmodules.md) — Boot image creation on Windows 11

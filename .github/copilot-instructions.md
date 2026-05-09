# Copilot Instructions — OSDeploy Docs

## Site Identity

| Field       | Value                                                          |
|-------------|----------------------------------------------------------------|
| Site name    | OSDeploy                                                                           |
| URL          | https://www.osdeploy.com/                                                          |
| Author       | David Segura                                                                       |
| Company      | Recast Software (https://www.recastsoftware.com)                                   |
| Platform     | GitBook (Markdown + GitBook shortcodes)                                            |
| Audience     | Windows sysadmins and IT deployment engineers                                      |
| Purpose      | Public website documenting how to use the OSDeploy PowerShell module               |
| Module repo  | [OSDeploy/RecastOSDeploy](https://github.com/OSDeploy/RecastOSDeploy)              |

## Writing Style

- Direct, technical, and practical. Avoid marketing language.
- Use imperative mood for instructions ("Run the following command", not "You should run…").
- PowerShell code blocks always use the `powershell` language identifier.
- First-person is acceptable for blog/archive posts; avoid it in reference docs.
- All PowerShell commands target **PowerShell 7.6 or later** (`pwsh`) unless explicitly noting Windows PowerShell 5.1.
- Windows target is **amd64 and arm64** — note architecture differences when relevant.

## GitBook Shortcodes

Use these GitBook-native shortcodes where appropriate:

```
{% hint style="info" %}   — informational callout
{% hint style="warning" %} — warning callout
{% hint style="danger" %}  — danger/critical callout
{% endhint %}

{% embed url="https://..." %}
{% endembed %}

{% file src="..." %}
```

Do **not** use standard Markdown blockquotes (`>`) as a replacement for `{% hint %}` blocks in new content.

## Formatting

All page content must follow the formatting guidelines defined in the `gitbook` skill at `.github/skills/gitbook/SKILL.md`. Load and apply that skill when writing or editing any page in this repository.

## PowerShell Modules

Three core modules are published to the PowerShell Gallery and are central to all OSDeploy workflows:

### OSDeploy

- **Gallery:** https://www.powershellgallery.com/packages/OSDeploy/
- **Primary use:** Used on **Windows 11 25H2** to create WinPE boot images.
- **Platform:** Windows 11 (amd64 / arm64), runs in a full Windows OS environment (not WinPE).
- **Status:** Current / actively maintained.
- **Install:**
  ```powershell
  Install-Module -Name OSDeploy -Force -SkipPublisherCheck
  ```

### OSDCloud

- **Gallery:** https://www.powershellgallery.com/packages/OSDCloud/
- **Primary use:** Used **in WinPE** to deploy Windows 11 to a device. This is the current, recommended module for OSDCloud functionality.
- **Platform:** WinPE (Windows Preinstallation Environment), amd64 and arm64.
- **Status:** Current / recommended.
- **Install (from WinPE or full OS):**
  ```powershell
  Install-Module -Name OSDCloud -Force -SkipPublisherCheck
  ```

### OSD

- **Gallery:** https://www.powershellgallery.com/packages/OSD/
- **Primary use:** Used **in WinPE** to deploy Windows 11. Also supports the older OSDCloud implementation (OSDCloud v1 / legacy).
- **Platform:** WinPE (Windows Preinstallation Environment).
- **Status:** Maintained. For OSDCloud functionality, the **OSDCloud module is preferred** over OSD.
- **Install:**
  ```powershell
  Install-Module -Name OSD -Force -SkipPublisherCheck
  ```

> Always use `-SkipPublisherCheck` when installing any of the three modules. `-Force` allows reinstall or upgrade without a confirmation prompt.

## Key Terminology

| Term          | Definition                                                                                                       |
|---------------|------------------------------------------------------------------------------------------------------------------|
| WinPE         | Windows Preinstallation Environment — a lightweight Windows boot environment used to prepare and deploy PCs.     |
| WinRE         | Windows Recovery Environment — the recovery partition boot environment built into Windows.                       |
| WinSE         | Windows Setup Environment — the environment used during Windows Setup from boot.wim.                             |
| Boot image    | A WIM file (typically `boot.wim` or `winpe.wim`) used to boot a device into WinPE.                              |
| ADK           | Windows Assessment and Deployment Kit — Microsoft's toolkit for creating and customizing Windows boot images.    |
| OSDCloud      | A cloud-based OS deployment method that downloads Windows images and OEM driver packs at runtime, no server needed. |
| OSDCloud v1   | The legacy OSDCloud implementation, included in the OSD module. Superseded by the OSDCloud module.              |
| DriverPack    | A vendor-provided (HP, Dell, Lenovo, Surface) archive of hardware drivers used during OS deployment.             |
| OOBE          | Out-of-Box Experience — the initial Windows setup flow shown after OS installation.                              |
| Autopilot     | Windows Autopilot — a Microsoft zero-touch provisioning service for configuring new or re-deployed Windows PCs.  |
| MDT           | Microsoft Deployment Toolkit — a legacy Microsoft tool for automated OS deployment.                              |
| ConfigMgr     | Microsoft Configuration Manager (SCCM) — enterprise endpoint management and OS deployment solution.             |
| PSCloudScript | A URL-based PowerShell script delivery mechanism used with OSDCloud for customization.                           |

## Module Version Notes

- The **OSDCloud** module supersedes the OSDCloud functionality that existed in the **OSD** module (v1).
- When writing content about OSDCloud, default to documenting the **OSDCloud** module unless the page explicitly covers legacy/v1 behavior.
- The **OSDeploy** module is for boot image *creation* on a full Windows OS, not for WinPE execution.

## Navigation Structure

The site is organized as follows:

| Section          | Content                                              |
|------------------|------------------------------------------------------|
| Start Here       | Prerequisites: PowerShell 7, PowerShell modules      |
| PowerShell Gallery | Module listings and links                          |
| OSDCloud         | OSDCloud deployment documentation                    |
| Inside WinPE     | WinPE internals: startup, boot environments, ADK     |
| Windows ADK      | ADK download and installation guides                 |
| OSDeployMDT      | MDT integration via the OSDeployMDT module           |
| Guides           | Autopilot, PSCloudScript, go OSDCloud                |
| Secure Boot      | Secure Boot reference links                          |
| WinPE Drivers    | Driver reference links                               |
| Archive          | Historical blog posts (2018–2022)                    |

## File Conventions

- All pages are Markdown (`.md`).
- Navigation is defined in `SUMMARY.md` — update it when adding new pages.
- Pages marked `hidden: true` in YAML front matter do not appear in the GitBook sidebar.
- Images are stored in `.gitbook/assets/` and referenced as `![](<.gitbook/assets/filename.png>)`.
- The `archive/` folder contains historical blog content — do not reorganize or rename archive files.

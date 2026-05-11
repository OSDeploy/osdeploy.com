# OSDeploy Home

OSDeploy is a PowerShell-driven platform for building WinPE boot images and deploying Windows 11 from the cloud. Three focused modules cover every phase of the deployment lifecycle — from creating a bootable image on Windows 11 to deploying an OS in WinPE.

{% hint style="info" %}
**OSDeploy and OSDCloud are releasing new modules — register for preview updates.**\
Sign up at [recastsoftware.com/osd-preview](https://www.recastsoftware.com/osd-preview/) to be notified when new builds drop and to participate in the preview program.
{% endhint %}

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

***

## Module Ecosystem

Three modules, three execution contexts — each one owns a distinct phase of the deployment pipeline.

| Module       | What it does                                 | Where it runs        | Status                |
| ------------ | -------------------------------------------- | -------------------- | --------------------- |
| **OSDeploy** | Builds and customizes WinPE boot images      | Windows 11 (full OS) | Preview               |
| **OSDCloud** | Deploys Windows 11 from cloud-hosted content | WinPE                | Current / Recommended |
| **OSD**      | Legacy OSDCloud v1 deployment                | WinPE                | Maintained (legacy)   |

***

## Get Started

The following workflow produces a fully customized WinPE boot image ready for OSDCloud deployment.

{% stepper %}
{% step %}
#### Install Prerequisites

Install all required and optional tools — Windows ADK 25H2 or 26H1, MDT, PowerShell 7, Git, VS Code, and Hyper-V — in a single command. Run in audit mode first (default), then add `-Force` to install.

```powershell
Install-OSDeploySoftware
```
{% endstep %}

{% step %}
#### Update WinPE Drivers

Pull in the latest WinPE driver packs for Dell, HP, Intel Ethernet, Intel Wi-Fi, and Microsoft Windows 25H2 features. Drivers are cached locally under `%ProgramData%\OSDeployCore`.

```powershell
Update-OSDeployWinPEDrivers
```
{% endstep %}

{% step %}
#### Import a Windows OS Image

Mount a Windows 11 ISO or installation media and import the OS image into OSDeployCore. The imported image is used as the WinRE source for your boot image build.

```powershell
Import-OSDeployOS
```
{% endstep %}

{% step %}
#### Build the Boot Image

Build a fully customized WinPE boot image: ADK optional components, OSDCloud module, WinPE apps (AzCopy, 7-Zip, curl, Sysinternals), wallpaper, console settings, and an exportable ISO — all in one command.

```powershell
Build-OSDeployBootMedia
```
{% endstep %}

{% step %}
#### Deploy Windows in WinPE

Boot the device from your ISO or USB drive. Once in WinPE, OSDCloud downloads Windows 11 and OEM driver packs directly from the cloud — no server required.

```powershell
Deploy-OSDCloud
```
{% endstep %}
{% endstepper %}

***

## Features

### Prerequisite Setup

`Install-OSDeploySoftware` audits and installs every tool the OSDeploy workflow depends on:

* **Windows ADK** 25H2 and 26H1 (with WinPE add-on)
* **Microsoft Deployment Toolkit** (MDT 6.3.8456)
* **PowerShell 7.6**, Git for Windows, Visual Studio Code, Hyper-V
* Runs in **audit mode by default** — add `-Force` to act
* Supports **amd64 and arm64** hosts

### WinPE Driver Library

`Update-OSDeployWinPEDrivers` and `Get-OSDeployWinPEDrivers` manage a local library of WinPE-compatible driver packs:

* **Dell** and **HP** WinPE driver packs
* **Intel Ethernet** and **Intel Wi-Fi** drivers
* **Microsoft Windows 25H2** Ethernet and Wi-Fi feature packages (driver store injection)
* **VMware** guest tools (arm64 supported)
* Interactive picker for manual selection, or full batch mode for automation

### Boot Image Building

`Build-OSDeployBootMedia` orchestrates a multi-phase WinPE image build:

* Mounts a WinRE or ADK WinPE source image
* Injects **22 ADK optional components** (PowerShell, .NET, WMI, DISM, Secure Boot cmdlets, and more)
* Adds **WinPE apps**: AzCopy (amd64 + arm64), 7-Zip 26.00, curl, BgInfo, Process Monitor
* Copies the **OSDCloud** and **OSD** PowerShell modules into the image
* Applies **console settings**, **custom wallpaper**, and **environment variables**
* Exports a bootable **ISO** and optionally writes to **USB**
* Supports **build profiles** for repeatable, version-controlled builds

### MDT Integration

`Install-OSDeployMDT` hooks OSDeploy into an existing MDT Deployment Share once. After that, every **Update Deployment Share** in MDT automatically:

* Injects ADK binaries and `oa3tool.exe`
* Installs PackageManagement, PowerShellGet, AzCopy, curl, and 7-Zip
* Copies the OSDCloud module into LiteTouchPE
* Adds WinPE drivers from the local driver library
* Patches the exported ISO for **UEFI CA 2023 Secure Boot** compatibility

### Hyper-V Test VMs

`New-OSDeployHyperVM` creates a Hyper-V VM pre-configured for OSDeploy testing:

* Full VM configuration with appropriate memory, disk, and network settings
* Auto-mounts the latest OSDeploy boot ISO
* Opens the VM console automatically
* VM checkpointing enabled by default

### Enterprise-Ready

* **`-WhatIf` and `-Confirm`** support on all state-changing functions
* **Non-interactive batch mode** for CI/CD and automated deployment pipelines
* **amd64 and arm64** throughout — both host build and WinPE target
* **Preview-first** release model — register at [recastsoftware.com/osd-preview](https://www.recastsoftware.com/osd-preview/)
* Single `Import-Module -Name OSDeploy` — no additional configuration required

***

## Quick Install

```powershell
# Build-time module — runs on Windows 11 to create boot images
Install-Module -Name OSDeploy -Force -SkipPublisherCheck

# WinPE deployment module — runs inside WinPE to deploy Windows 11
Install-Module -Name OSDCloud -Force -SkipPublisherCheck

# Legacy OSDCloud v1 support (optional)
Install-Module -Name OSD -Force -SkipPublisherCheck
```

***

## OSDeploy Function Reference

The OSDeploy module (v0.1.0 preview) exports 15 public functions across six areas.

### Module Utilities

| Function                    | Description                                              |
| --------------------------- | -------------------------------------------------------- |
| `Get-OSDeployModulePath`    | Returns the file system path to the OSDeploy module root |
| `Get-OSDeployModuleVersion` | Returns the currently loaded OSDeploy module version     |
| `Invoke-OSDeployHydration`  | Runs the full OSDeploy hydration workflow end-to-end     |

### Boot Media

| Function                  | Description                                                           |
| ------------------------- | --------------------------------------------------------------------- |
| `Build-OSDeployBootMedia` | Builds a customized WinPE boot image from a WinRE or ADK WinPE source |
| `Update-OSDeployISO`      | Rebuilds bootable ISO files for an existing BootImage build           |
| `Import-OSDeployOS`       | Imports Windows OS images from installation media into OSDeployCore   |
| `New-OSDeployUSB`         | Creates a new bootable OSDeploy USB drive                             |
| `Update-OSDeployUSB`      | Updates an existing OSDeploy USB drive with new BootMedia             |

### WinPE Drivers

| Function                      | Description                                                    |
| ----------------------------- | -------------------------------------------------------------- |
| `Get-OSDeployWinPEDrivers`    | Lists available WinPE driver packs in the local driver library |
| `Update-OSDeployWinPEDrivers` | Downloads and caches the latest WinPE driver packs             |

### Software Installation

| Function                   | Description                                             |
| -------------------------- | ------------------------------------------------------- |
| `Install-OSDeploySoftware` | Installs OS deployment prerequisite software on Windows |

### MDT Integration

| Function              | Description                                                         |
| --------------------- | ------------------------------------------------------------------- |
| `Install-OSDeployMDT` | Initializes an MDT Deployment Share for OSDeploy                    |
| `Invoke-OSDeployMDT`  | MDT LiteTouchPE exit script — runs on every Update Deployment Share |

### Virtual Machines

| Function              | Description                                              |
| --------------------- | -------------------------------------------------------- |
| `New-OSDeployHyperVM` | Creates a Hyper-V VM pre-configured for OSDeploy testing |

***

## What's New

### v0.1.0 — April 27, 2026 — Initial Preview Release

* **Core:** `Get-OSDeployModulePath`, `Get-OSDeployModuleVersion`
* **Software:** `Install-OSDeploySoftware` — ADK 25H2/26H1, MDT, pwsh, Git, VS Code, Hyper-V
* **WinPE Drivers:** `Get-OSDeployWinPEDrivers`, `Update-OSDeployWinPEDrivers`
* **Boot Media:** `Import-OSDeployOS`, `Build-OSDeployBootMedia`, `New-OSDeployUSB`, `Update-OSDeployISO`, `Update-OSDeployUSB`
* **MDT:** `Install-OSDeployMDT`, `Invoke-OSDeployMDT`
* **Hyper-V:** `New-OSDeployHyperVM`
* MAML help, PlatyPS docs, `PSDefaultParameterValues` two-layer configuration system
* Platforms: Windows 11 25H2+, PowerShell 7.6+, amd64 and arm64

{% hint style="info" %}
Register at [recastsoftware.com/osd-preview](https://www.recastsoftware.com/osd-preview/) to get notified when new preview builds drop.
{% endhint %}

***

## Upcoming Events

{% hint style="info" %}
**May 22, 2026** — Recast Right Click Tools Release Show — OSDCloud Monthly Feature Release\
**Jun–Oct 2026** — Monthly Recast Right Click Tools Release Shows\
See the full [Events](events.md) calendar for session links and details.
{% endhint %}

***

## Explore This Site

* [**Core Components**](core-components/about.md) — Prerequisites: PowerShell 7, Windows ADK, MDT, WinPE drivers, and developer tools
* [**PowerShell Modules**](powershell-modules/about.md) — OSDeploy, OSDCloud, and OSD module reference
* [**OSDeploy MDT Integration**](osdeploy-mdt-integration/about.md) — Hook OSDeploy into MDT for automated LiteTouchPE modernization
* [**Inside WinPE**](inside-winpe/winpe-startup/) — WinPE startup sequence, boot environments, and troubleshooting
* [**Guides**](guides/autopilot-app-registration.md) — Autopilot app registration, PSCloudScript, and go OSDCloud workflows
* [**Events**](events.md) — Conference sessions, webinars, and release shows

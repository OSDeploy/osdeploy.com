# Windows Assessment and Deployment Kit (ADK)

The Windows ADK is Microsoft's toolkit for creating and customizing Windows Preinstallation Environment (WinPE) boot images. OSDeploy uses the ADK — specifically the **WinPE add-on** — to build and modify boot images on Windows 11.

| Property | Value |
|---|---|
| Publisher | Microsoft |
| Current version | 10.1.26100.x (Windows 11 26H1) |
| Architecture | amd64 / arm64 |
| Platform | Windows 11 |
| Required by | `Build-OSDeployBootMedia`, `Update-OSDeployBootMedia` |

{% embed url="https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install" %}
{% endembed %}

## Overview

The Windows ADK provides the command-line tools and Windows PE files needed to create bootable WinPE images. The ADK itself includes tools such as DISM and the Deployment Tools; the **WinPE add-on** is a separate download that provides the actual WinPE boot files. Both must be installed together.

OSDeploy supports the two most recent ADK releases that align with Windows 11 25H2 and 26H1. The version of the ADK must match the Windows release you intend to build WinPE for. Architecture-specific builds (amd64 and arm64) are supported.

## Why OSDeploy Requires the ADK

- Creates the WinPE mount point and boot image structure that OSDeploy populates
- Provides `dism.exe` for mounting, injecting drivers, and committing WinPE images
- Supplies the Windows PE base components (kernel, shell, networking, scripting host)
- Required on the Windows 11 build machine — not used at deployment time in WinPE

{% hint style="info" %}
The ADK and its WinPE add-on must be the same version. Install them on the Windows 11 machine where you run `Build-OSDeployBootMedia` — not in WinPE itself.
{% endhint %}

## In This Section

- [Install 25H2](install-25h2.md) — Install the Windows ADK for Windows 11 25H2
- [Install 26H1](install-26h1.md) — Install the Windows ADK for Windows 11 26H1

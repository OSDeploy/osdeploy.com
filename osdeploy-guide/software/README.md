---
description: Install-OSDeploySoftware
---

# Software Installation

## Quick Setup

Windows ADK 25H2 and 7-Zip are required for the standard OSDeploy and OSDCloud workflow. Run these commands from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

Preview the required software installation without making changes:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip'
```

Install the required software:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip' -Force
```

## Overview

`Install-OSDeploySoftware` is the single entry point for preparing the software used by an OSDeploy PC. Use it to discover supported components, review their sources, install one or more components, and cache supported installers for later use.

{% hint style="info" %}
Only Windows ADK 25H2 and 7-Zip are required for OSDeploy and OSDCloud. All other components are optional and should be installed only when the OSDeploy PC or workflow needs them.
{% endhint %}

{% hint style="info" %}
Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`. These requirements are checked before list, preview, download, and install operations.
{% endhint %}

## How the Function Works

Run the function without `-Name` to list the available components:

```powershell
Install-OSDeploySoftware
```

Specify a name to preview its source, documentation links, and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2'
```

Add `-Force` to install a component. Multiple components are processed in the order specified.

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip' -Force
```

Use `-DownloadOnly` to download supported content without installing it on the OSDeploy PC.

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip' -DownloadOnly
```

## Software

| Name            | Software                                     | Use case    | Method                      | Download only |
| --------------- | -------------------------------------------- | ----------- | --------------------------- | ------------- |
| `adk-25h2`      | Windows ADK 10.1.26100.2454 and WinPE add-on | Required    | `curl.exe` and vendor setup | Yes           |
| `7zip`          | 7-Zip and the 7-Zip WinPE files              | Required    | WinGet and GitHub           | Yes           |
| `adk-26h1`      | Windows ADK 10.1.28000.1 and WinPE add-on    | Special use | `curl.exe` and vendor setup | Yes           |
| `mdt`           | Microsoft Deployment Toolkit 6.3.8456.1000   | Special use | Verified MSI                | Yes           |
| `git`           | Git for Windows                              | Optional    | WinGet                      | No            |
| `code`          | Visual Studio Code                           | Optional    | WinGet                      | No            |
| `code-insiders` | Visual Studio Code Insiders                  | Optional    | WinGet                      | No            |
| `hyperv`        | Hyper-V                                      | Optional    | Windows optional feature    | No            |

{% hint style="warning" %}
Windows ADK 26H1 is a special-use component. Do not install it unless the OSDeploy PC is running Windows 11 26H1. Use Windows ADK 25H2 for the standard OSDeploy and OSDCloud workflow.
{% endhint %}

{% hint style="warning" %}
Microsoft no longer supports MDT. Its installer is provided only as a convenience for existing workflows that still require it. Do not install MDT unless there is a specific need. See the [MDT retirement notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement).
{% endhint %}

## OSDeployCore

The function keeps reusable installers and WinPE application files in `C:\ProgramData\OSDeployCore`. This allows later OSDeploy operations to reuse the downloaded content.

```
C:\ProgramData\OSDeployCore\
|-- software\
|   |-- Microsoft.WindowsADK_10.1.26100.2454\
|   |   |-- adk\
|   |   `-- adkwinpe\
|   |-- Microsoft.WindowsADK_10.1.28000.1\
|   |-- Microsoft.DeploymentToolkit_6.3.8456.1000\
|   `-- 7zip.7zip\
|       |-- amd64\
|       `-- arm64\
`-- cache\
  `-- winpe-apps\
    `-- 7zip\
      `-- <version>\
```

| Component                                                     | Content added to OSDeployCore                                                                                                                                                                                                                     |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Windows ADK 25H2                                              | Saves `adksetup.exe` below `software\Microsoft.WindowsADK_10.1.26100.2454\adk` and `adkwinpesetup.exe` below `software\Microsoft.WindowsADK_10.1.26100.2454\adkwinpe`. A full installation also downloads each offline layout into these folders. |
| Windows ADK 26H1                                              | Saves `adksetup.exe` and `adkwinpesetup.exe` below `software\Microsoft.WindowsADK_10.1.28000.1`.                                                                                                                                                  |
| Microsoft Deployment Toolkit                                  | Saves the verified `MicrosoftDeploymentToolkit_x64.msi` below `software\Microsoft.DeploymentToolkit_6.3.8456.1000`.                                                                                                                               |
| 7-Zip                                                         | Downloads amd64 and arm64 installers below `software\7zip.7zip`, then prepares versioned `7zr.exe` and 7-Zip Extra files below `cache\winpe-apps\7zip`.                                                                                           |
| Git, Visual Studio Code, Visual Studio Code Insiders, Hyper-V | Does not add reusable content to OSDeployCore. WinGet manages the application downloads, and Hyper-V is a Windows optional feature.                                                                                                               |

`-DownloadOnly` is supported for both ADK releases, MDT, and 7-Zip. It is not supported for Git, either Visual Studio Code channel, or Hyper-V.

## Component Guides

* [Windows ADK 25H2](windows-adk-25h2.md)
* [Windows ADK 26H1](windows-adk-26h1.md)
* [Microsoft Deployment Toolkit](microsoft-mdt.md)
* [Git for Windows](git-for-windows.md)
* [Visual Studio Code](vs-code-stable.md)
* [Visual Studio Code Insiders](vs-code-insiders.md)
* [Microsoft Hyper-V](microsoft-hyper-v.md)
* [7-Zip](7-zip.md)

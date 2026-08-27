# Install-OSDeploySoftware

Installs OS deployment prerequisite software on Windows.

| Property | Value                                                                  |
|----------|------------------------------------------------------------------------|
| Module   | OSDeploy                                                               |
| Platform | Windows 11 (amd64 / arm64)                                            |
| Requires | PowerShell 7.6; Administrator rights for ADK, MDT, and Hyper-V installs; winget for Git and VS Code |
| Output   | `System.Management.Automation.PSCustomObject`                          |

## Description

Single entry point for setting up a Windows workstation with all tools commonly required for OS deployment work.

Without `-Force`, the function runs in **preview mode**: it returns a table showing each requested component's name, download source, documentation link, and the exact command needed to install it.

Add `-Force` to perform the actual installation.

Supported components:

| Name           | Full Name                                       | Install Method        |
|----------------|-------------------------------------------------|-----------------------|
| `adk-25h2`     | Windows ADK 10.1.26100.2454 + WinPE add-on     | `curl.exe` download   |
| `adk-26h1`     | Windows ADK 10.1.28000.1 + WinPE add-on        | `curl.exe` download   |
| `mdt`          | Microsoft Deployment Toolkit 6.3.8456.1000      | `curl.exe` download   |
| `git`          | Git for Windows                                 | winget                |
| `code`         | Visual Studio Code (stable)                     | winget                |
| `code-insiders`| Visual Studio Code Insiders (pre-release)       | winget                |
| `hyperv`       | Hyper-V Windows optional feature                | `Enable-WindowsOptionalFeature` |
| `7zip`         | 7-Zip                                           | winget                |

When `-Name` is omitted, the function returns a list of all available components and the exact command to install each one. No changes are made.

## Syntax

```powershell
Install-OSDeploySoftware [[-Name] <String[]>] [-Force] [-DownloadOnly] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter       | Type       | Required | Description                                                                                                                                      |
|-----------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Name`         | `String[]` | No       | One or more component names to inspect or install. Accepts alias `-Component`. When omitted, lists all available components.                    |
| `-Force`        | `Switch`   | No       | Performs the installation. Without this switch the command runs in preview mode only.                                                            |
| `-DownloadOnly` | `Switch`   | No       | Downloads installers to the cache without installing. Not supported for winget- or feature-based components (`git`, `code`, `code-insiders`, `hyperv`). |

## Examples

```powershell
# List all available components and the install command for each
Install-OSDeploySoftware
```

```powershell
# Preview what would be installed for Windows ADK 26H1
Install-OSDeploySoftware -Name 'adk-26h1'
```

```powershell
# Install Windows ADK 26H1 and its WinPE add-on
Install-OSDeploySoftware -Name 'adk-26h1' -Force
```

```powershell
# Install Git for Windows, Visual Studio Code, and VS Code Insiders
Install-OSDeploySoftware -Name 'git', 'code', 'code-insiders' -Force
```

```powershell
# Install 7-Zip
Install-OSDeploySoftware -Name '7zip' -Force
```

```powershell
# Download the MDT installer without installing
Install-OSDeploySoftware -Name 'mdt' -DownloadOnly
```

```powershell
# Install MDT
Install-OSDeploySoftware -Name 'mdt' -Force
```

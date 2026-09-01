---
description: >-
  Install Windows ADK 26H1 and its matching Windows PE add-on for applicable
  OSDeploy builds.
---

# Windows ADK 26H1

Use the exact name `adk-26h1` to download or install Windows ADK `10.1.28000.1` and its matching Windows PE add-on. These components provide deployment and WinPE files for workflows that require the 26H1 kit.

{% hint style="warning" %}
Use Windows ADK 25H2 for the standard OSDeploy and OSDCloud workflow. Select the 26H1 component only when the intended boot-image workflow requires that kit.
{% endhint %}

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, and internet access. It uses the ADK and WinPE setup URLs in current module metadata. The helper does not expose an architecture selector or branch on host architecture.

## Preview

Return the ADK setup URL, documentation links, and install command without creating a cache directory, downloading setup programs, or inspecting installed ADK versions:

```powershell
Install-OSDeploySoftware -Name 'adk-26h1'
```

## Install

Download the setup programs and install the ADK when no Windows ADK is registered:

```powershell
Install-OSDeploySoftware -Name 'adk-26h1' -Force
```

When no ADK is detected, the helper silently installs these features directly from the setup programs:

* `OptionId.DeploymentTools`
* `OptionId.ImagingAndConfigurationDesigner`
* `OptionId.WindowsPreinstallationEnvironment` from the Windows PE add-on

The setup programs are retained in:

```
C:\ProgramData\OSDeployCore\software\Microsoft.WindowsADK_10.1.28000.1\
```

Unlike the 25H2 helper, the 26H1 helper does not create offline layout content before installation. The setup processes run quietly with CEIP disabled and `/norestart`. Any nonzero download, ADK, or WinPE setup exit code stops the command.

{% hint style="info" %}
If any Windows ADK version is registered, the helper downloads the two setup programs but keeps the installed version and skips installation. It still creates the missing x86 `WinPE_OCs` directory used by the MDT Windows PE MMC snap-in when needed.
{% endhint %}

## Download Only

Download the ADK and Windows PE setup programs without installing them:

```powershell
Install-OSDeploySoftware -Name 'adk-26h1' -DownloadOnly
```

This saves `adksetup.exe` and `adkwinpesetup.exe` in the versioned OSDeployCore software folder. It does not install features or apply the x86 workaround.

## WhatIf and Result

Add `-WhatIf` to `-Force` or `-DownloadOnly` to report the parent action without invoking the ADK helper. No cache directory or setup files are created in that mode.

A completed `-Force` action returns `Component` set to `Windows ADK 26H1` and `Status` set to `Installed`. A completed `-DownloadOnly` action sets `Status` to `Downloaded`. The parent result does not include setup paths, exit codes, detected ADK version, or whether installation was skipped. The setup commands do not request a restart.

## Verify

Confirm the registered ADK and Windows PE files:

```powershell
Get-ItemProperty `
  'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*', `
  'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*' |
  Where-Object DisplayName -Like 'Windows Assessment and Deployment Kit*' |
  Select-Object DisplayName, DisplayVersion

Test-Path 'C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment'
```

## Next Step

Install [7-Zip](7-zip.md), then continue with [Build-OSDeployBoot](../build-osdeployboot.md) when the intended source and kit align. See the [Windows ADK 26H1 reference](/broken/pages/Bx8J8y9gkS8gpEo5vMqc) for the broader ADK setup context.

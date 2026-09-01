---
description: Install Windows ADK 25H2 and its matching Windows PE add-on for OSDeploy boot-image creation.
---

# Windows ADK 25H2

Use the exact name `adk-25h2` to download or install Windows ADK `10.1.26100.2454` and its matching Windows PE add-on. These components provide the deployment and WinPE files used to create standard OSDeploy boot images.

<figure><img src="../../../.gitbook/assets/image (190).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Windows ADK 25H2 is required for the standard OSDeploy and OSDCloud workflow.
{% endhint %}

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, and internet access. It uses the ADK and WinPE setup URLs in current module metadata. The helper does not expose an architecture selector or branch on host architecture.

## Preview

Return the ADK setup URL, documentation links, and install command without creating cache directories, downloading setup programs, or inspecting installed ADK versions:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2'
```

## Install

Download the setup programs and install the ADK when no Windows ADK is registered:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -Force
```

When no ADK is detected, the helper creates offline layouts and silently installs these features:

* `OptionId.DeploymentTools`
* `OptionId.ImagingAndConfigurationDesigner`
* `OptionId.WindowsPreinstallationEnvironment` from the Windows PE add-on

The setup programs and offline layout content are retained under:

```text
C:\ProgramData\OSDeployCore\software\Microsoft.WindowsADK_10.1.26100.2454\
```

The setup processes run quietly with CEIP disabled and `/norestart`. Any nonzero download, layout, ADK, or WinPE setup exit code stops the command.

{% hint style="info" %}
If any Windows ADK version is registered, the helper downloads the two setup programs but keeps the installed version and skips layout creation and installation. It still creates the missing x86 `WinPE_OCs` directory used by the MDT Windows PE MMC snap-in when needed.
{% endhint %}

## Download Only

Download the ADK and Windows PE setup programs without installing them:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -DownloadOnly
```

This saves `adksetup.exe` under the `adk` subfolder and `adkwinpesetup.exe` under the `adkwinpe` subfolder. It does not create the complete offline layouts or apply the x86 workaround.

## WhatIf and Result

Add `-WhatIf` to `-Force` or `-DownloadOnly` to report the parent action without invoking the ADK helper. No cache directories or setup files are created in that mode.

A completed `-Force` action returns `Component` set to `Windows ADK 25H2` and `Status` set to `Installed`. A completed `-DownloadOnly` action sets `Status` to `Downloaded`. The parent result does not include setup paths, exit codes, detected ADK version, or whether installation was skipped. The setup commands do not request a restart.

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

Install [7-Zip](7-zip.md), then continue with [Build-OSDeployBoot](../build-osdeployboot.md). See the [Windows ADK 25H2 reference](../../../core-components/microsoft-windows-adk/install-25h2.md) for the broader ADK setup context.

---
description: >-
  Install Windows ADK 26H1 and its Windows PE add-on with
  Install-OSDeploySoftware.
---

# Windows ADK 26H1

Windows ADK 26H1 provides the Deployment Tools, Windows Configuration Designer, and Windows PE files used to create OSDeploy boot images. `Install-OSDeploySoftware` installs ADK version `10.1.28000.1` and its matching Windows PE add-on.

{% hint style="warning" %}
Windows ADK 26H1 is a special-use component. Do not install it unless the OSDeploy PC is running Windows 11 26H1. Use Windows ADK 25H2 for the standard OSDeploy and OSDCloud workflow.
{% endhint %}

## Preview

Review the configured download source and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'adk-26h1'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name 'adk-26h1' -Force
```

The function silently installs these ADK features:

* Deployment Tools
* Imaging and Configuration Designer
* Windows Preinstallation Environment

The setup programs are retained in:

```
C:\ProgramData\OSDeployCore\software\Microsoft.WindowsADK_10.1.28000.1\
```

The function also creates the missing x86 `WinPE_OCs` directory required by MDT when necessary.

{% hint style="info" %}
If any Windows ADK version is already installed, the function keeps that version and skips the ADK installation. It still applies the x86 `WinPE_OCs` fix when needed.
{% endhint %}

## Download Only

Download the ADK and Windows PE setup programs without installing them:

```powershell
Install-OSDeploySoftware -Name 'adk-26h1' -DownloadOnly
```

This saves `adksetup.exe` and `adkwinpesetup.exe` in the versioned OSDeployCore software folder.

## Related

* [Install Software](../../basic-setup/software.md)
* [Windows ADK 26H1 reference](../../../core-components/microsoft-windows-adk/install-26h1.md)

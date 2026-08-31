---
description: >-
  Install Windows ADK 25H2 and its Windows PE add-on with
  Install-OSDeploySoftware.
---

# Windows ADK 25H2

Windows ADK 25H2 provides the Deployment Tools, Windows Configuration Designer, and Windows PE files used to create OSDeploy boot images. `Install-OSDeploySoftware` installs ADK version `10.1.26100.2454` and its matching Windows PE add-on.

<figure><img src="../../../.gitbook/assets/image (190).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Windows ADK 25H2 is required for the standard OSDeploy and OSDCloud workflow.
{% endhint %}

## Preview

Review the configured download source and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -Force
```

The function downloads offline layouts and silently installs these ADK features:

* Deployment Tools
* Imaging and Configuration Designer
* Windows Preinstallation Environment

The downloaded content is retained in:

```
C:\ProgramData\OSDeployCore\software\Microsoft.WindowsADK_10.1.26100.2454\
```

The function also creates the missing x86 `WinPE_OCs` directory required by MDT when necessary.

{% hint style="info" %}
If any Windows ADK version is already installed, the function keeps that version and skips the ADK installation. It still applies the x86 `WinPE_OCs` fix when needed.
{% endhint %}

## Download Only

Download the ADK and Windows PE setup programs without installing them:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -DownloadOnly
```

This saves `adksetup.exe` in the `adk` subfolder and `adkwinpesetup.exe` in the `adkwinpe` subfolder. It does not download the complete offline layouts.

## Related

* [Install Software](../../basic-setup/software.md)
* [Windows ADK 25H2 reference](../../../core-components/microsoft-windows-adk/install-25h2.md)

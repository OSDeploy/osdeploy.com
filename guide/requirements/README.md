---
description: >-
  Prepare a Windows 11 PC with the software required to build OSDeploy boot
  media.
---

# PC Requirements

Prepare a Windows 11 workstation with PowerShell 7, Hyper-V, and the modules required to create and test OSDeploy boot media.

{% hint style="warning" %}
Do not use the default WinGet command to install PowerShell. Beginning with PowerShell 7.6.0, WinGet installs the unsupported MSIX package by default. Install PowerShell from the MSI package.
{% endhint %}

## Complete the Setup

{% stepper %}
{% step %}
### Confirm the Requirements

Use a PC with:

* Windows 11 25H2, build 26200 or later
* amd64 architecture
* Local administrative rights
* At least 50 GB of free space on the system volume
* Internet access

Windows 11 on arm64 can potentially be used, but it is not fully tested. Other Windows client versions and Windows Server are unsupported.
{% endstep %}

{% step %}
### Update Windows and Enable Hyper-V

Open Windows PowerShell as an administrator, open Windows Update, and install all available cumulative updates:

```powershell
Start-Process 'ms-settings:windowsupdate'
```

On a physical Windows 11 Pro, Enterprise, or Education PC with hardware virtualization enabled, enable Hyper-V:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All' -All -NoRestart
```

Restart Windows before continuing.
{% endstep %}

{% step %}
### Install PowerShell 7

Download the latest stable amd64 or arm64 `.msi` from [PowerShell releases](https://github.com/PowerShell/PowerShell/releases/latest). Run the MSI and enable the recommended installation options, including adding PowerShell to `PATH` and enabling Microsoft Update.

Open PowerShell 7 as an administrator by running `pwsh`. Confirm that the session uses PowerShell Core 7.6 or later and that `$PSHOME` is under `$env:ProgramFiles\PowerShell\7`:

```powershell
$PSVersionTable | Select-Object PSEdition, PSVersion
$PSHOME
```
{% endstep %}

{% step %}
### Install the Required Modules

Install `OSDeploy` and `OSDCloud` from the elevated PowerShell 7 session:

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

Install `OSD` only when a workflow requires legacy OSD or OSDCloud v1 commands.
{% endstep %}

{% step %}
### Register the OSDeploy PC

Complete [Community Registration](../registration.md) and confirm that `Show-OSDeployLicense` returns a valid Recast Software Community License. Registration is required while the OSDeploy module is in preview.
{% endstep %}
{% endstepper %}

See the detailed instructions for [Windows 11](windows-11-os.md), [PowerShell 7](powershell-7.md), and [PowerShell modules](../../osdeploy-guide/requirements/powershell-modules.md).

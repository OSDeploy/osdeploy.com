---
description: Install Windows ADK 25H2 and 7-Zip for OSDeploy boot-image creation.
---

# Install Required Software

Use `Install-OSDeploySoftware` to install Windows ADK 25H2, its WinPE add-on, and 7-Zip. These components provide the servicing tools and archive support required to build OSDeploy boot media.

## Install the Required Software

{% stepper %}
{% step %}
### Confirm the Requirements

Run the function on a workstation that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../requirements/powershell-modules.md)
* Administrator rights
* `curl.exe` available in `PATH`
* Internet access

{% hint style="warning" %}
The function stops before previewing or installing the software when a Windows, PowerShell, `curl.exe`, or administrator requirement is not met.
{% endhint %}
{% endstep %}

{% step %}
### Preview the Installation

Open an elevated PowerShell 7.6 session and preview both components without making changes:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip'
```

Review the returned source, installation details, and commands for Windows ADK 25H2 and 7-Zip.
{% endstep %}

{% step %}
### Install the Software

Install both components:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip' -Force
```

The function downloads and installs Windows ADK 25H2 and its matching WinPE add-on. It also installs 7-Zip and prepares the amd64 and arm64 7-Zip files used in OSDeploy boot images.
{% endstep %}
{% endstepper %}

For installation and download behavior, see [Windows ADK 25H2](../cmdlets/install-osdeploysoftware/windows-adk-25h2.md) and [7-Zip](../cmdlets/install-osdeploysoftware/7-zip.md).

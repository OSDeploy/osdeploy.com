---
description: >-
  Prepare an OSDeploy workstation, build boot media, and test the completed
  environment.
---

# Basic Setup

Use these steps to quickly prepare a Windows 11 workstation, build OSDeploy boot media, and create or test the media. Run the commands from an elevated PowerShell 7.6 or later session.

## Complete the Basic Setup

{% stepper %}
{% step %}
### [Install Required Software](install-osdeploysoftware.md)

Install Windows ADK 25H2, its WinPE add-on, and 7-Zip:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip' -Force
```
{% endstep %}

{% step %}
### [Create OSDeployCore](update-osdeploycore.md)

Download and prepare the Windows, recovery, and driver content used by OSDeploy:

```powershell
Update-OSDeployCore
```
{% endstep %}

{% step %}
### [Build an OSDeploy Boot Image](build-boot.md)

Create customized WinPE media and ISO files:

```powershell
Build-OSDeployBoot -Name 'MyPE'
```
{% endstep %}

{% step %}
### [Create a new OSDeploy USB](new-osdeploybootusb.md)

Create a bootable USB drive from a completed build:

```powershell
New-OSDeployBootUSB
```

{% hint style="danger" %}
This command removes every partition and all data from the selected USB disk. Verify the disk number, model, and size before approving the clear operation.
{% endhint %}
{% endstep %}

{% step %}
### [Test OSDeploy Boot in Hyper-V](new-osdeployhypervm.md)

Create and start a Hyper-V virtual machine from the newest OSDeploy boot ISO:

```powershell
New-OSDeployHyperVM
```
{% endstep %}
{% endstepper %}

---
description: Enable Microsoft Hyper-V with Install-OSDeploySoftware.
---

# Microsoft Hyper-V

Hyper-V provides the local virtualization platform used to test OSDeploy boot images and deployment workflows. `Install-OSDeploySoftware` enables the `Microsoft-Hyper-V-All` Windows optional feature and all parent features.

{% hint style="info" %}
Hyper-V is optional. Install it only when the OSDeploy PC will run virtual machines for deployment testing.
{% endhint %}

## Preview

Review the feature name and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'hyperv'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session on a physical OSDeploy PC running Windows 11:

```powershell
Install-OSDeploySoftware -Name 'hyperv' -Force
```

The function enables Hyper-V without automatically restarting Windows. Restart the OSDeploy PC when the returned `RestartNeeded` property is `True`.

Verify the feature state:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All'
```

{% hint style="warning" %}
OSDeploy skips Hyper-V when it detects that the OSDeploy PC is a virtual machine.
{% endhint %}

Hyper-V is a Windows optional feature. This component does not add files to OSDeployCore and does not support `-DownloadOnly`.

## Related

* [Install Software](./)
* [Windows 11](../operating-system/windows-11-os.md)

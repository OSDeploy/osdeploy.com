---
description: Install Microsoft Deployment Toolkit with Install-OSDeploySoftware.
---

# Microsoft MDT

Microsoft Deployment Toolkit (MDT) provides deployment shares, task sequences, and scripts used by existing MDT-based workflows. `Install-OSDeploySoftware` downloads, verifies, and installs MDT version `6.3.8456.1000`.

<figure><img src="../../.gitbook/assets/image (319).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Microsoft no longer supports MDT. This installer is provided only as a convenience for existing workflows that still require it. Do not install MDT unless there is a specific need. Review the [Microsoft retirement notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement).
{% endhint %}

## Preview

Review the configured MSI source and retirement notice without making changes:

```powershell
Install-OSDeploySoftware -Name 'mdt'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name 'mdt' -Force
```

The function downloads `MicrosoftDeploymentToolkit_x64.msi`, validates its SHA256 hash against the OSDeploy module metadata, and installs it silently with no automatic restart.

The verified MSI is retained in:

```
C:\ProgramData\OSDeployCore\software\Microsoft.DeploymentToolkit_6.3.8456.1000\
```

If MDT is already installed, the function reports a version mismatch when applicable and does not replace the installed version.

## Download Only

Download and verify the MSI without installing MDT:

```powershell
Install-OSDeploySoftware -Name 'mdt' -DownloadOnly
```

## Related

* [Install Software](./)
* [Microsoft Deployment Toolkit reference](../../core-components/microsoft-deployment-toolkit/install-mdt.md)

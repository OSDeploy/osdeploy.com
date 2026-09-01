---
description: >-
  Download, verify, and install the retired Microsoft Deployment Toolkit for
  existing workflows.
---

# Microsoft MDT

Use the exact name `mdt` to download, verify, or install Microsoft Deployment Toolkit `6.3.8456.1000`. The component is retained for existing MDT deployment shares and integrations.

<figure><img src="../../../.gitbook/assets/image (319).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Microsoft no longer supports MDT. This installer is provided only as a convenience for existing workflows that still require it. Do not install MDT unless there is a specific need. Review the [Microsoft retirement notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement).
{% endhint %}

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, `msiexec.exe`, and internet access. It always downloads the x64 file `MicrosoftDeploymentToolkit_x64.msi`; there is no architecture selector.

## Preview

Return the configured MSI source, retirement link, and install command without creating the cache or inspecting installed MDT versions:

```powershell
Install-OSDeploySoftware -Name 'mdt'
```

## Install

Download and verify the configured MSI, then install MDT when no MDT product is registered:

```powershell
Install-OSDeploySoftware -Name 'mdt' -Force
```

The helper checks both 64-bit and 32-bit uninstall registry locations for a product whose display name begins with `Microsoft Deployment Toolkit`. It reports a version mismatch when the registered version differs from `6.3.8456.1000` but does not replace any detected MDT installation.

The MSI is downloaded with `curl.exe`, validated against the SHA256 value in current OSDeploy module metadata, and installed by `msiexec.exe` with `/qn /norestart`. A checksum mismatch or nonzero MSI exit code stops the command. The installer does not request an automatic restart.

The verified MSI is retained in:

```
C:\ProgramData\OSDeployCore\software\Microsoft.DeploymentToolkit_6.3.8456.1000\
```

## Download Only

Download and verify the MSI without installing MDT:

```powershell
Install-OSDeploySoftware -Name 'mdt' -DownloadOnly
```

The action reuses the cached MSI only when its SHA256 hash matches the module metadata.

## WhatIf and Result

Add `-WhatIf` to `-Force` or `-DownloadOnly` to report the parent action without invoking the MDT helper. The versioned cache directory is not created in that mode.

A completed `-Force` action returns `Component` set to `Microsoft Deployment Toolkit` and `Status` set to `Installed`. A completed `-DownloadOnly` action sets `Status` to `Downloaded`. The parent result does not indicate whether MDT was newly installed, skipped because a version was already present, or downloaded to a particular path.

## Verify

Confirm the registered product and inspect the verified installer:

```powershell
Get-ItemProperty `
	'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*', `
	'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*' |
	Where-Object DisplayName -Like 'Microsoft Deployment Toolkit*' |
	Select-Object DisplayName, DisplayVersion

Get-FileHash `
	'C:\ProgramData\OSDeployCore\software\Microsoft.DeploymentToolkit_6.3.8456.1000\MicrosoftDeploymentToolkit_x64.msi' `
	-Algorithm SHA256
```

## Next Step

Continue with [Install-OSDeployMDT](../install-osdeploymdt.md) when an existing workflow requires OSDeploy integration. See the [Microsoft Deployment Toolkit reference](/broken/pages/8QBjU6vMYXdxHlWOnpo3) for the broader setup context.

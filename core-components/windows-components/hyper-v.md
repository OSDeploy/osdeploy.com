# Hyper-V

Hyper-V is a Windows optional feature that enables virtual machine creation on the build machine. OSDeploy uses it to test WinPE boot images locally without physical hardware.

| Property | Value |
|---|---|
| Publisher | Microsoft |
| Feature name | `Microsoft-Hyper-V-All` |
| Architecture | amd64 / arm64 |
| Platform | Windows 11 |
| Required by | `New-OSDeployHyperVM` |

{% embed url="https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v" %}
{% endembed %}

## Overview

Hyper-V is included in Windows 11 Pro, Enterprise, and Education editions but is not enabled by default. Enabling it allows `New-OSDeployHyperVM` to create a virtual machine that boots from a WinPE image built by OSDeploy, making it possible to validate boot images and test OSDCloud deployments entirely on the build machine. A system restart is required after enabling Hyper-V before it is operational.

## Why OSDeploy Uses Hyper-V

- `New-OSDeployHyperVM` creates a test VM that boots from a WinPE boot image built by OSDeploy
- Enables local validation of boot images before deploying to physical devices
- Eliminates the need for physical test hardware during development and testing
- Required only for VM-based testing — not needed for boot image creation or OSDCloud deployments

{% hint style="warning" %}
A **system restart is required** after enabling Hyper-V before virtual machines can be created or started. Save all work and plan for a restart before enabling.
{% endhint %}

---

## Enable with OSDeploy

The OSDeploy module enables the Hyper-V Windows optional feature using `Enable-WindowsOptionalFeature`. Administrator rights are required.

**Preview (no changes made):**

```powershell
Install-OSDeploySoftware -Name 'hyperv'
```

**Enable Hyper-V:**

```powershell
Install-OSDeploySoftware -Name 'hyperv' -Force
```

{% hint style="info" %}
`-DownloadOnly` is not supported for feature-based components. `-Force` is required to make any change.
{% endhint %}

---

## Enable Manually

Enable Hyper-V from PowerShell or via the Windows Features dialog.

**PowerShell (requires Administrator):**

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All' -All -NoRestart
```

**Windows Features dialog:**

1. Open **Control Panel** → **Programs** → **Turn Windows features on or off**.
2. Expand **Hyper-V** and ensure all sub-items are checked.
3. Click **OK** and restart when prompted.

---

## Related

- [Enable Hyper-V on Windows](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)
- [Install-OSDeploySoftware](https://docs.osdeploy.com)
- [New-OSDeployHyperVM](https://docs.osdeploy.com)

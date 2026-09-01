---
description: Enable the Microsoft Hyper-V optional feature and identify whether Windows must restart.
---

# Microsoft Hyper-V

Use the exact name `hyperv` to enable the `Microsoft-Hyper-V-All` Windows optional feature and its parent features. Hyper-V is optional and supports local virtual-machine testing of OSDeploy media and deployment workflows.

## Requirements

Run the command on a physical workstation from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The parent command requires the current OSDeploy module and `curl.exe`, even though enabling Hyper-V does not download an installer. Windows must provide `Get-WindowsOptionalFeature` and `Enable-WindowsOptionalFeature`.

## Preview

Return the feature name, documentation links, and install command without inspecting or enabling the feature:

```powershell
Install-OSDeploySoftware -Name 'hyperv'
```

## Install

Enable Hyper-V when its state is not already `Enabled` or `EnablePending`:

```powershell
Install-OSDeploySoftware -Name 'hyperv' -Force
```

The helper runs `Enable-WindowsOptionalFeature` with `-All` and `-NoRestart`, then reads the feature state again. It does not restart Windows automatically.

{% hint style="warning" %}
When the parent command detects a virtual machine, it skips the Hyper-V helper and returns `Status` set to `NotSupported`. Nested Hyper-V installation is not attempted.
{% endhint %}

## Download Only

Hyper-V is a Windows feature and has no separate installer to download:

```powershell
Install-OSDeploySoftware -Name 'hyperv' -DownloadOnly
```

On a physical workstation, this returns `Action` set to `DownloadOnly`, `Status` set to `NotSupported`, and a note.

## WhatIf and Result

Use `-Force -WhatIf` to report the enable action without invoking the Hyper-V helper.

A completed action returns `Component` set to `Hyper-V`, `Status` set to `Installed`, and `RestartNeeded`. `Status` does not distinguish newly enabled Hyper-V from a feature that was already enabled. Restart Windows when `RestartNeeded` is `True`.

Inside a virtual machine, the skip result contains `Name`, `Component`, `Action`, `Status`, and `Note` instead.

## Verify

Confirm the feature state after any required restart:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All'
```

## Next Step

After Hyper-V is operational, use [New-OSDeployHyperVM](../new-osdeployhypervm.md) to create an OSDeploy test virtual machine. Review the [Windows 11 requirement](../../requirements/windows-11-os.md) when preparing the host.

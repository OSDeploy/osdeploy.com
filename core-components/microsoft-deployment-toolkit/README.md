# Microsoft Deployment Toolkit

Microsoft Deployment Toolkit (MDT) is a legacy framework for automating Windows OS deployment. OSDeploy integrates with MDT via the **OSDeployMDT** module to support existing MDT-based deployment workflows.

{% hint style="warning" %}
Microsoft Deployment Toolkit has been officially retired by Microsoft. Install it only if you have an existing MDT-based workflow that requires it.

[MDT Retirement Notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement)
{% endhint %}

| Property | Value |
|---|---|
| Publisher | Microsoft |
| Final version | 6.3.8456.1000 |
| Architecture | x64 |
| Platform | Windows |
| Status | Retired — no new releases |
| Required by | `Install-OSDeployMDT`, `Invoke-OSDeployMDT` |

{% embed url="https://learn.microsoft.com/en-us/intune/configmgr/mdt/" %}
{% endembed %}

## Overview

MDT provides a framework for creating deployment shares, task sequences, and automated OS installs. It was the dominant Windows deployment tool before cloud-based solutions and Microsoft Intune became mainstream. The final release (6.3.8456.1000) remains functional on current Windows versions but receives no further updates from Microsoft.

OSDeploy's OSDeployMDT integration targets shops that already run MDT deployment shares and want to extend or automate them with PowerShell without rebuilding their entire deployment infrastructure.

## Why OSDeploy Requires MDT

- `Install-OSDeployMDT` configures and extends an existing MDT deployment share
- `Invoke-OSDeployMDT` runs automatically as the MDT LiteTouchPE exit script on every Update Deployment Share
- MDT's `Microsoft.BDD.TaskSequencer` COM objects are used for task sequence orchestration
- Required only for MDT integration features — not needed for standalone WinPE or OSDCloud deployments

{% hint style="info" %}
If you are starting a new deployment workflow, use OSDCloud instead of MDT. MDT integration in OSDeploy exists to support existing environments.
{% endhint %}

## In This Section

- [Install MDT](install-mdt.md) — Download, verify, and install the final MDT release

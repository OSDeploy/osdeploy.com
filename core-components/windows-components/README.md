# Windows Components

Windows Components are optional Windows features that enable specific OSDeploy workflows on the build machine.

| Property | Value |
|---|---|
| Publisher | Microsoft |
| Platform | Windows 11 |
| Architecture | amd64 / arm64 |
| Install method | `Install-OSDeploySoftware` or Windows Optional Features |

{% embed url="https://learn.microsoft.com/en-us/windows/client-management/client-tools/add-remove-hide-features" %}
{% endembed %}

## Overview

Windows 11 ships with several features that are available but not enabled by default. OSDeploy uses **Hyper-V** to create and manage virtual machines on the build machine, enabling local testing of WinPE boot images without physical hardware. Enabling Windows optional features requires Administrator rights and may require a system restart.

## Why OSDeploy Uses Windows Components

- Hyper-V enables `New-OSDeployHyperVM` to create test VMs that boot from a WinPE image built by OSDeploy
- Local VM testing allows rapid iteration on boot images before deploying to physical devices
- Hyper-V is not required for building boot images — only for running them in a local VM

## In This Section

- [Hyper-V](hyper-v.md) — Enable the Hyper-V optional feature for VM-based boot media testing

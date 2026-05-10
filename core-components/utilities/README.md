# Utilities

Utilities are additional tools that OSDeploy downloads and uses during boot image creation. They are installed on the Windows 11 build machine and, where applicable, injected directly into WinPE.

| Component | Purpose                                              | Required  | Install Method             |
|-----------|------------------------------------------------------|-----------|----------------------------|
| 7-Zip     | Extraction and archive utility; injected into WinPE  | Optional  | `Install-OSDeploySoftware` |

{% hint style="info" %}
Use `Install-OSDeploySoftware` to install utilities. Run it without parameters to list all available components and the command to install each one.
{% endhint %}

## Overview

Utilities complement the core ADK and driver workflows. Some utilities run only on the build machine (for example, to extract driver packs); others are injected into the WinPE image so they are available at deployment time. OSDeploy manages utility downloads and WinPE injection automatically during `Build-OSDeployBootMedia` and `Invoke-OSDeployMDT`.

## In This Section

- [7-Zip](7zip.md) — Archive extraction tool installed on the build machine and injected into WinPE

# PowerShell

PowerShell 7.6 or later is required for all OSDeploy workflows. It installs side-by-side with Windows PowerShell 5.1 and is launched using `pwsh`.

| Property         | Value                                            |
| ---------------- | ------------------------------------------------ |
| Publisher        | Microsoft (open source)                          |
| Required version | 7.6 or later                                     |
| Executable       | `pwsh`                                           |
| Architecture     | amd64 / arm64                                    |
| Platform         | Windows 11                                       |
| Required by      | All OSDeploy, OSDCloud, and OSD module workflows |

{% embed url="https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6" %}

## Overview

PowerShell 7 (also called PowerShell Core) is the cross-platform, open-source successor to Windows PowerShell 5.1. It ships as a separate executable (`pwsh`) installed to `$env:ProgramFiles\PowerShell\7` and does not replace or modify the Windows PowerShell 5.1 installation. OSDeploy requires 7.6 specifically because it aligns with the minimum version tested for module compatibility and includes stability improvements for system-level operations.

## Why OSDeploy Requires PowerShell 7.6+

* The OSDeploy PowerShell Module was authored and tested against PowerShell 7.6. OSD and OSDCloud PowerShell modules require Windows PowerShell 5.1.
* Modern PowerShell APIs (`ForEach-Object -Parallel`, improved error handling) are used throughout
* `pwsh` runs in a full .NET runtime with access to system APIs needed by DISM-based operations
* OSDCloud deployments in WinPE require `pwsh` — Windows PowerShell 5.1 is not available in OSDCloud WinPE images

{% hint style="warning" %}
Do **not** install PowerShell 7 via WinGet or the Microsoft Store. Beginning with PowerShell 7.6.0, WinGet installs the MSIX package by default. MSIX packages run inside an application sandbox that blocks system-level operations, including `dism.exe`. Use the **MSI installer** for all OSDeploy scenarios.
{% endhint %}

## In This Section

* [Install PowerShell 7](install-powershell.md) — Download and silently install the MSI package for amd64 or arm64
* [PowerShell Modules](../../powershell-modules/osdeploy/) — Install the OSDeploy, OSDCloud, and OSD modules from the PowerShell Gallery
* [OSD Module](../../powershell-modules/osd-module.md) — Details for the OSD module (legacy OSDCloud v1 support)
* [OSDCloud Module](../../powershell-modules/osdcloud-module.md) — Details for the OSDCloud module (current, recommended)

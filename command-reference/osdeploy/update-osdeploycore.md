# Update-OSDeployCore

Updates all OSDeployCore assets: Windows Enterprise ESD files, OS images, and WinPE driver packages.

| Property | Value                                                             |
|----------|-------------------------------------------------------------------|
| Module   | OSDeploy                                                          |
| Platform | Windows 11 (amd64 / arm64)                                       |
| Requires | PowerShell 7.6, Run as Administrator, internet access, valid license |
| Output   | Mixed `System.IO.FileInfo` and `System.IO.DirectoryInfo` child output |

## Description

Runs `Update-OSDeployCoreESD`, `Update-OSDeployCoreRE`, and `Update-OSDeployCoreDrivers` in a single call to refresh all local OSDeployCore assets.

Use this function to bring a new build machine up to date or to ensure all cached content is current before running `Build-OSDeployBoot`.

## Syntax

```powershell
Update-OSDeployCore [-WhatIf] [-Confirm]
```

## Parameters

| Parameter  | Type     | Required | Description                                                              |
|------------|----------|----------|--------------------------------------------------------------------------|
| `-WhatIf`  | `Switch` | No       | Flows preview behavior to the three stage commands. Core initialization, cache inspection, prompts, and network discovery can still occur. |
| `-Confirm` | `Switch` | No       | Flows confirmation preference to each stage; the orchestrator has no single workflow confirmation. |

## Examples

```powershell
# Download any missing ESD files, OS images, and WinPE driver packages
Update-OSDeployCore
```

```powershell
# Preview what would be downloaded without executing
Update-OSDeployCore -WhatIf
```

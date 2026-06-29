# Update-OSDeployCore

Updates all OSDeployCore assets: Windows Enterprise ESD files, OS images, and WinPE driver packages.

| Property | Value                                                             |
|----------|-------------------------------------------------------------------|
| Module   | OSDeploy                                                          |
| Platform | Windows 11 (amd64 / arm64)                                       |
| Requires | PowerShell 7.6, Run as Administrator, internet access             |

## Description

Runs `Update-OSDeployCoreESD`, `Update-OSDeployCoreOS`, and `Update-OSDeployCoreDrivers` in a single call to refresh all local OSDeployCore assets.

Use this function to bring a new build machine up to date or to ensure all cached content is current before running `Build-OSDeployBoot`.

## Syntax

```powershell
Update-OSDeployCore [-WhatIf] [-Confirm]
```

## Parameters

| Parameter  | Type     | Required | Description                                                              |
|------------|----------|----------|--------------------------------------------------------------------------|
| `-WhatIf`  | `Switch` | No       | Shows what each sub-function would do without performing any downloads.  |
| `-Confirm` | `Switch` | No       | Prompts for confirmation before running.                                 |

## Examples

```powershell
# Download any missing ESD files, OS images, and WinPE driver packages
Update-OSDeployCore
```

```powershell
# Preview what would be downloaded without executing
Update-OSDeployCore -WhatIf
```

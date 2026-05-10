# Build-OSDeployHydra

Runs the complete OSDeploy hydration workflow end-to-end on a supported Windows 11 machine.

| Property | Value                                                                   |
|----------|-------------------------------------------------------------------------|
| Module   | OSDeploy                                                                |
| Platform | Windows 11 25H2 or 26H1 (amd64 / arm64)                                |
| Requires | PowerShell 7.6, Run as Administrator                                    |

## Description

Automates the full OSDeploy setup sequence:

1. Detects the running OS version and processor architecture.
2. Installs the matching Windows ADK, Git for Windows, Visual Studio Code, VS Code Insiders, and 7-Zip.
3. Downloads WinPE drivers for the detected architecture.
4. Builds a WinPE boot image named `Hydra`, using the newest available WinRE source when present, or falling back to the ADK WinPE when none is found.

Without `-Force`, interactive pickers (Out-GridView) are shown at each step. With `-Force`, the entire sequence runs non-interactively from start to finish.

## Syntax

```powershell
Build-OSDeployHydra [-Force] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type     | Required | Description                                                                                                  |
|-----------|----------|----------|--------------------------------------------------------------------------------------------------------------|
| `-Force`  | `Switch` | No       | Runs all steps non-interactively. Skips Out-GridView pickers and processes all matching sources automatically. |

## Examples

```powershell
# Run the full hydration workflow with interactive pickers at each step
Build-OSDeployHydra
```

```powershell
# Run the full hydration workflow non-interactively
Build-OSDeployHydra -Force
```

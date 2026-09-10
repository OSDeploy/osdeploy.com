# Invoke-OSDeployHydration

Runs the complete OSDeploy workstation hydration workflow.

| Property | Value |
| --- | --- |
| Module | OSDeploy |
| Platform | Windows 11 25H2 build 26200 or later (amd64 / arm64) |
| Requires | PowerShell 7.6, `curl.exe`, OSDCloud, Administrator rights |
| Output | Mixed child-command output: `PSCustomObject`, `FileInfo`, `DirectoryInfo`, and `String` |

## Syntax

```powershell
Invoke-OSDeployHydration [-Force] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-Force` | `Switch` | No | Bypass hydration-level `ShouldContinue` prompts, install offered components, and forward force behavior to ESD and driver updates. It is not passed to CoreRE, Build, or Hyper-V creation. |
| `-WhatIf` | `Switch` | No | Preview delegated `ShouldProcess` operations. It does not suppress `ShouldContinue` prompts unless `-Force` is also used. |
| `-Confirm` | `Switch` | No | Flow confirmation preference into delegated commands. |

## Examples

```powershell
Invoke-OSDeployHydration
```

```powershell
Invoke-OSDeployHydration -Force
```

## Workflow

The command validates the workstation, displays the planned workflow, and uses console Yes/No prompts unless `-Force` is supplied. It then:

1. Tests and optionally installs ADK 25H2, 7-Zip, Git, VS Code, VS Code Insiders, and Hyper-V.
2. Runs `Update-OSDeployCoreESD -Architecture $arch`.
3. Runs `Update-OSDeployCoreRE -Architecture $arch`.
4. Runs `Update-OSDeployCoreDrivers -Architecture $arch`.
5. Runs `Build-OSDeployBoot -Auto`.
6. On a physical host, optionally creates a Hyper-V VM when Hyper-V and the new ISO are available.

Hydration is the immediate-caller exception for child OSDeploy license gates. It does not use `Out-GridView` for its own confirmation prompts, and `-Force` does not guarantee that all delegated selectors or child confirmations are suppressed.

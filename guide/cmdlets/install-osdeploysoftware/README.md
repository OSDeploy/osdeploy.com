---
description: List, preview, download, or install the software components used by OSDeploy.
---

# Install-OSDeploySoftware

`Install-OSDeploySoftware` is the entry point for preparing an OSDeploy workstation. Use it to list supported components, inspect current module metadata, download supported installers, or run one or more component installers.

## Requirements

Run the command from an elevated PowerShell session on a workstation that meets all of these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../../requirements/powershell-modules.md)
* `curl.exe` available in `PATH`
* Internet access for downloads and installations

Components installed through WinGet also require `winget.exe`, supplied by App Installer. MDT additionally requires `msiexec.exe`; Hyper-V requires the Windows optional-feature cmdlets.

{% hint style="warning" %}
The command checks Windows, the Windows build, PowerShell, the PowerShell installation type, `curl.exe`, and administrator access before it lists or previews components. It stops without returning component data when any check fails.
{% endhint %}

## Components

Use one or more of these exact values with `-Name`:

| Name | Component | Installer or action | Architecture behavior |
| --- | --- | --- | --- |
| `adk-25h2` | Windows ADK 25H2 `10.1.26100.2454` and Windows PE add-on | Microsoft setup programs | Uses the setup URLs in current module metadata. |
| `adk-26h1` | Windows ADK 26H1 `10.1.28000.1` and Windows PE add-on | Microsoft setup programs | Uses the setup URLs in current module metadata. |
| `mdt` | Microsoft Deployment Toolkit `6.3.8456.1000` | x64 MSI | Downloads `MicrosoftDeploymentToolkit_x64.msi`. |
| `git` | Git for Windows | WinGet package `Git.Git` | The helper does not pass an architecture argument to WinGet. |
| `code` | Visual Studio Code | WinGet package `Microsoft.VisualStudioCode` | The helper does not pass an architecture argument to WinGet. |
| `code-insiders` | Visual Studio Code Insiders | WinGet package `Microsoft.VisualStudioCode.Insiders` | The helper does not pass an architecture argument to WinGet. |
| `hyperv` | Microsoft Hyper-V | Windows optional feature `Microsoft-Hyper-V-All` | Uses the feature available in the running Windows installation. |
| `7zip` | 7-Zip | WinGet package `7zip.7zip` | Downloads both x64 and arm64 installers for the OSDeploy cache. |

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-Name` | `String[]` | None | Accepts the eight component names above and the alias `-Component`. Omit it to list every component. A supplied name previews by default. This parameter does not accept pipeline input. |
| `-Force` | `Switch` | Not enabled | Runs the selected installers or enables the selected feature. Without `-Force` or `-DownloadOnly`, the command returns preview metadata. |
| `-DownloadOnly` | `Switch` | Not enabled | Downloads supported ADK, MDT, or 7-Zip content without installing it. When combined with `-Force`, download-only behavior takes precedence. Returns `NotSupported` for `git`, `code`, `code-insiders`, and `hyperv`. |
| `-WhatIf` | Common parameter | Not enabled | Runs the shared requirement checks and reports each requested download or installation action without invoking its component helper. |
| `-Confirm` | Common parameter | Not enabled | Prompts separately for each requested download or installation action. The command uses `ConfirmImpact Medium`. |

## Examples

### List available components

Return every accepted name and its preview command without selecting a component:

```powershell
Install-OSDeploySoftware
```

### Preview multiple components

Return module metadata and install commands for Windows ADK 25H2 and 7-Zip without downloading or installing them:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip'
```

### Install multiple components

Install Git for Windows and both Visual Studio Code channels in the supplied order:

```powershell
Install-OSDeploySoftware -Name 'git', 'code', 'code-insiders' -Force
```

Git installation can prompt for the current user's global Git name and email address when those values are missing.

### Download supported installers

Download and verify the MDT MSI without installing MDT:

```powershell
Install-OSDeploySoftware -Name 'mdt' -DownloadOnly
```

### Preview an installation action with WhatIf

Run requirement checks and show the Hyper-V action without enabling the feature:

```powershell
Install-OSDeploySoftware -Name 'hyperv' -Force -WhatIf
```

### Capture action results

Install Windows ADK 25H2 and 7-Zip, then inspect the parent command's component status objects:

```powershell
$result = Install-OSDeploySoftware -Name 'adk-25h2', '7zip' -Force
$result | Format-Table
```

## Action Behavior

The command selects its mode from the supplied parameters:

1. With no `-Name`, it lists all supported components.
2. With `-Name` but without `-Force` or `-DownloadOnly`, it returns preview metadata from the current OSDeploy module data.
3. With `-DownloadOnly`, it downloads supported component content. This mode takes precedence when `-Force` is also present.
4. With `-Force`, it invokes each selected installer or feature action in the order supplied to `-Name`.

The command stops on a terminating component error. Changes completed by earlier components are not rolled back. An action result with `Status` set to `Installed` means the component helper completed; it does not distinguish a new installation from an already-present component.

## Component Behavior

* ADK setup downloads the ADK and matching Windows PE bootstrapper. Installation adds Deployment Tools, Imaging and Configuration Designer, and Windows Preinstallation Environment. Any detected ADK version causes the requested ADK installation to be skipped.
* MDT downloads an x64 MSI, validates its SHA256 hash, and installs it silently with no automatic restart. An existing MDT installation is not replaced.
* Git, Visual Studio Code, and Visual Studio Code Insiders install through WinGet only when their command is not found. Git can also prompt for missing global identity values.
* Hyper-V enables `Microsoft-Hyper-V-All` with all parent features and no automatic restart. The command returns `NotSupported` instead of invoking the helper when it detects a virtual machine.
* 7-Zip downloads x64 and arm64 installers, installs the host application when needed, and prepares the versioned 7-Zip WinPE app cache.

See the child component guides for cache paths, verification commands, and component-specific restrictions.

## WhatIf and Confirmation

`Install-OSDeploySoftware` calls `ShouldProcess` once for each selected component immediately before invoking its helper. `-WhatIf` still runs the shared requirement checks and writes the OSDeploy banner, but it does not download, install, enable, or cache component content. Because no helper runs, the command returns no action result for that component.

`-Confirm` prompts at the same boundary. Declining one component omits its result and continues to the next selected component. After accepting the parent prompt, components that implement their own `ShouldProcess` checks can display additional confirmation prompts for their internal changes.

Listing and preview modes do not call `ShouldProcess`; `-WhatIf` does not change their returned data.

## Output

The command returns `System.Management.Automation.PSCustomObject` instances. The properties depend on the selected mode.

### Component list

| Property | Description |
| --- | --- |
| `Name` | Exact value accepted by `-Name`. |
| `FullName` | Display name of the component. |
| `Action` | `Install`. |
| `Command` | Preview command for the component. |

### Preview

| Property | Description |
| --- | --- |
| `Name` | Selected `-Name` value. |
| `Component` | Display name of the component. |
| `Action` | `Preview`. |
| `Source` | Package ID, feature name, or download URL from current module metadata. |
| `Docs` | Primary documentation URL. |
| `Details` | Additional release, retirement, or component information URL. |
| `Note` | Explains how to install or download the component. |
| `Command` | Installation command using `-Force`. |

### Download and installation actions

Successful ADK, MDT, Git, Visual Studio Code, and 7-Zip actions return `Component` and `Status`. `Status` is `Downloaded` for download-only actions and `Installed` for installation actions. Hyper-V also returns `RestartNeeded`.

Unsupported download-only requests and Hyper-V requests inside a virtual machine return `Name`, `Component`, `Action`, `Status`, and `Note`; `Status` is `NotSupported`.

Start with [Install Required Software](../../basic/install-osdeploysoftware.md) for the standard workstation setup, or use the [Install-OSDeploySoftware command reference](../../../command-reference/osdeploy/install-osdeploysoftware.md) for compact syntax.

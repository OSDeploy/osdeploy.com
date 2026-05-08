# Visual Studio Code

Visual Studio Code is a lightweight, extensible code editor used for authoring and debugging PowerShell scripts for OSDeploy workflows. Both the stable and Insiders (pre-release) channels are available.

| Property | Value |
|---|---|
| Publisher | Microsoft |
| Stable winget ID | `Microsoft.VisualStudioCode` |
| Insiders winget ID | `Microsoft.VisualStudioCode.Insiders` |
| Platform | Windows 11 |
| Architecture | amd64 / arm64 |
| Required by | Optional — script authoring |
| Status | Current |

{% embed url="https://code.visualstudio.com/docs/setup/windows" %}
{% endembed %}

## Overview

Visual Studio Code provides IntelliSense, integrated terminal, debugging, and source control for PowerShell script development. The **PowerShell extension** adds full language support for `.ps1` files, including syntax highlighting, parameter completion, and script analysis via PSScriptAnalyzer. The Insiders channel receives updates daily and is useful for testing the latest features.

## Why VS Code Is Useful for OSDeploy

- Full PowerShell editing with parameter and module completion
- Integrated terminal runs `pwsh` directly in the workspace
- Built-in Git integration for tracking script changes
- Extensions available for editing JSON module configuration files (`module.json`)

---

## Install with OSDeploy

Both stable and Insiders editions are installed via `winget`.

**Preview stable channel (no changes made):**

```powershell
Install-OSDeploySoftware -Name 'code'
```

**Install stable channel:**

```powershell
Install-OSDeploySoftware -Name 'code' -Force
```

**Install Insiders channel:**

```powershell
Install-OSDeploySoftware -Name 'code-insiders' -Force
```

**Install both channels:**

```powershell
Install-OSDeploySoftware -Name 'code', 'code-insiders' -Force
```

{% hint style="info" %}
`winget` (App Installer) must be available. Install it from the [Microsoft Store](https://apps.microsoft.com/detail/9NBLGGH4NNS1) if it is not present. `-DownloadOnly` is not supported for winget-based components.
{% endhint %}

---

## Manual Installation

**Install stable via winget:**

```powershell
winget install --id Microsoft.VisualStudioCode -e -h --accept-source-agreements --accept-package-agreements
```

**Install Insiders via winget:**

```powershell
winget install --id Microsoft.VisualStudioCode.Insiders -e -h --accept-source-agreements --accept-package-agreements
```

**Download from the VS Code website:**

Visit [https://code.visualstudio.com/download](https://code.visualstudio.com/download) and select the Windows installer for your architecture (x64 or ARM64).

**Verify the installation:**

```powershell
code --version
```

---

## Recommended Extensions

After installation, add the PowerShell extension for full language support:

```powershell
code --install-extension ms-vscode.powershell
```

---

## Related

- [Visual Studio Code documentation](https://code.visualstudio.com/docs/setup/windows)
- [VS Code release notes](https://code.visualstudio.com/updates)
- [PowerShell extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)
- [Install-OSDeploySoftware](https://docs.osdeploy.com)
- [Git for Windows](git.md) — integrated source control

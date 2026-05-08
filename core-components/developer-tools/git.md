# Git for Windows

Git for Windows is a source control system for tracking changes to deployment scripts and configurations. OSDeploy installs it via `winget` and optionally configures the global Git identity.

| Property | Value |
|---|---|
| Publisher | The Git Project |
| winget ID | `Git.Git` |
| Platform | Windows 11 |
| Architecture | amd64 / arm64 |
| Required by | Optional — script source control |
| Status | Current |

{% embed url="https://git-scm.com/download/win" %}
{% endembed %}

## Overview

Git for Windows provides the `git` command-line tool and a Bash environment on Windows. It integrates with GitHub, Azure DevOps, and any other Git host. After installation, `Install-OSDeploySoftware` will interactively prompt for a global `user.email` and `user.name` if they are not already configured, or walk through updating existing values.

## Why Git Is Useful for OSDeploy

- Track changes to OSDeploy build scripts and PSCloudScript files across time
- Push configuration to GitHub for sharing and collaboration
- Enables pulling the latest scripts from a repository inside WinPE (with internet access)
- Required if you use VS Code's built-in source control panel

---

## Install with OSDeploy

Git for Windows is installed via `winget`. After installation, the function prompts for global Git identity configuration.

**Preview (no changes made):**

```powershell
Install-OSDeploySoftware -Name 'git'
```

**Install:**

```powershell
Install-OSDeploySoftware -Name 'git' -Force
```

{% hint style="info" %}
`winget` (App Installer) must be available. Install it from the [Microsoft Store](https://apps.microsoft.com/detail/9NBLGGH4NNS1) if it is not present. `-DownloadOnly` is not supported for winget-based components.
{% endhint %}

---

## Manual Installation

**Install via winget:**

```powershell
winget install --id Git.Git -e -h --accept-source-agreements --accept-package-agreements
```

**Configure global Git identity after installation:**

```powershell
git config --global user.email 'you@example.com'
git config --global user.name 'Your Name'
```

**Verify the installation:**

```powershell
git --version
git config --global --list
```

---

## Related

- [Git for Windows](https://git-scm.com/download/win)
- [winget documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
- [Install-OSDeploySoftware](https://docs.osdeploy.com)
- [Visual Studio Code](vscode.md) — integrated Git support

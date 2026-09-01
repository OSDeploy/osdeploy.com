---
description: Install Git for Windows and complete the current user's global Git identity.
---

# Git for Windows

Use the exact name `git` to install the `Git.Git` WinGet package. Git is optional for OSDeploy execution, but it is useful when the workstation clones or maintains repositories.

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, `winget.exe`, and internet access. The helper does not pass an architecture argument to WinGet.

## Preview

Return the WinGet package ID, documentation links, and install command without installing Git or changing Git configuration:

```powershell
Install-OSDeploySoftware -Name 'git'
```

## Install

Install Git when the `git` command is unavailable:

```powershell
Install-OSDeploySoftware -Name 'git' -Force
```

After installation or detection, the helper reads the current user's global `user.email` and `user.name`. It prompts for values when either setting is missing and offers to change existing values. These prompts are part of the current `Install-OSDeploySoftware` path; the parent command does not expose the helper's noninteractive or identity parameters.

{% hint style="warning" %}
Plan for interactive Git identity prompts. The approved values persist in the current user's global Git configuration.
{% endhint %}

Git installation is silent. The command refreshes the current process environment and also checks `C:\Program Files\Git\cmd\git.exe` when `git` is not immediately available in `PATH`. It stops when WinGet fails or when the installed command still cannot be found.

## Download Only

Git does not support the parent command's download-only mode:

```powershell
Install-OSDeploySoftware -Name 'git' -DownloadOnly
```

This returns `Action` set to `DownloadOnly`, `Status` set to `NotSupported`, and a note. It does not invoke WinGet or change Git identity.

## WhatIf and Result

Use `-Force -WhatIf` to report the installation action without invoking the Git helper. It does not install Git or prompt for identity values.

A completed `-Force` action returns `Component` set to `Git for Windows` and `Status` set to `Installed`. The parent result does not expose the Git version, identity values, or whether Git was newly installed. The installer does not request a restart.

## Verify

Confirm the command and global identity:

```powershell
git --version
git config --global user.name
git config --global user.email
```

## Next Step

Use Git to clone or maintain the required repositories. See the [Git for Windows reference](/broken/pages/E4jBLgkQxDXMcZPQxuly) for the related OSDeploy setup guidance.

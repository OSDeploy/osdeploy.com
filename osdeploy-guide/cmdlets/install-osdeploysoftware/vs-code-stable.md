---
description: Install the stable Visual Studio Code channel with WinGet and OSDeploy.
---

# Visual Studio Code

Use the exact name `code` to install the stable `Microsoft.VisualStudioCode` WinGet package. Visual Studio Code is optional, but it provides an editor for OSDeploy PowerShell scripts and repository content.

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, `winget.exe`, and internet access. The helper does not pass an architecture argument to WinGet.

## Preview

Return the WinGet package ID, documentation links, and install command without installing Visual Studio Code:

```powershell
Install-OSDeploySoftware -Name 'code'
```

## Install

Install Visual Studio Code when the `code` command cannot be found:

```powershell
Install-OSDeploySoftware -Name 'code' -Force
```

The helper first searches `PATH`, the standard per-user path under `%LOCALAPPDATA%`, and the standard per-machine path under `%ProgramFiles%`. When no command is found, WinGet runs a silent installation that:

* Adds file and folder Explorer context-menu commands.
* Associates supported files.
* Adds the `code` command to `PATH`.
* Does not start Visual Studio Code after installation.

The command refreshes the current process environment and searches for `code` again. It stops on a nonzero WinGet exit code or when the installed command still cannot be found.

## Download Only

Visual Studio Code does not support the parent command's download-only mode:

```powershell
Install-OSDeploySoftware -Name 'code' -DownloadOnly
```

This returns `Action` set to `DownloadOnly`, `Status` set to `NotSupported`, and a note. It does not invoke WinGet.

## WhatIf and Result

Use `-Force -WhatIf` to report the installation action without invoking the Visual Studio Code helper.

A completed `-Force` action returns `Component` set to `Visual Studio Code` and `Status` set to `Installed`. The parent result does not include the detected version, command path, or whether the package was newly installed. The installer does not request a restart.

## Verify

Verify the installation:

```powershell
Get-Command 'code'
code --version | Select-Object -First 1
```

## Next Step

Open the OSDeploy repository or scripts in Visual Studio Code. See the [Visual Studio Code reference](/broken/pages/bX8Yla26fUSdecquBoN5) for related editor setup.

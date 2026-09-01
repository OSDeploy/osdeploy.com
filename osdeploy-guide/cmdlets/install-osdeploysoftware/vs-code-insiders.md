---
description: Install the Visual Studio Code Insiders channel with WinGet and OSDeploy.
---

# Visual Studio Code Insiders

Use the exact name `code-insiders` to install the `Microsoft.VisualStudioCode.Insiders` WinGet package. This optional channel provides prerelease Visual Studio Code builds and can be installed beside the stable channel.

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, `winget.exe`, and internet access. The helper does not pass an architecture argument to WinGet.

## Preview

Return the WinGet package ID, documentation links, and install command without installing Visual Studio Code Insiders:

```powershell
Install-OSDeploySoftware -Name 'code-insiders'
```

## Install

Install Visual Studio Code Insiders when the `code-insiders` command cannot be found:

```powershell
Install-OSDeploySoftware -Name 'code-insiders' -Force
```

The helper first searches `PATH`, the standard per-user path under `%LOCALAPPDATA%`, and the standard per-machine path under `%ProgramFiles%`. When no command is found, WinGet runs a silent installation that:

* Adds file and folder Explorer context-menu commands.
* Associates supported files.
* Adds the `code-insiders` command to `PATH`.
* Does not start Visual Studio Code Insiders after installation.

The command refreshes the current process environment and searches for `code-insiders` again. It stops on a nonzero WinGet exit code or when the installed command still cannot be found.

## Download Only

Visual Studio Code Insiders does not support the parent command's download-only mode:

```powershell
Install-OSDeploySoftware -Name 'code-insiders' -DownloadOnly
```

This returns `Action` set to `DownloadOnly`, `Status` set to `NotSupported`, and a note. It does not invoke WinGet.

## WhatIf and Result

Use `-Force -WhatIf` to report the installation action without invoking the Visual Studio Code Insiders helper.

A completed `-Force` action returns `Component` set to `Visual Studio Code Insiders` and `Status` set to `Installed`. The parent result does not include the detected version, command path, or whether the package was newly installed. The installer does not request a restart.

## Verify

Verify the installation:

```powershell
Get-Command 'code-insiders'
code-insiders --version | Select-Object -First 1
```

## Next Step

Open the OSDeploy repository or scripts in Visual Studio Code Insiders. See the [Visual Studio Code reference](/broken/pages/bX8Yla26fUSdecquBoN5) for related editor setup.

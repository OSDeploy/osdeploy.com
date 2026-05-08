# Developer Tools

Developer Tools are optional software tools for authoring, testing, and customizing OSDeploy workflows on the build machine.

| Property | Value |
|---|---|
| Platform | Windows 11 |
| Architecture | amd64 / arm64 |
| Install method | `Install-OSDeploySoftware` or vendor installer |
| Required | Optional |

## Overview

None of the tools in this section are required to build a WinPE boot image or run an OSDCloud deployment. They are recommended for anyone who writes or customizes OSDeploy scripts, manages configuration files in source control, or prefers an integrated editor for PowerShell development. Both Git and Visual Studio Code can be installed from a single `Install-OSDeploySoftware` command.

## Why These Tools Are Useful

- **Git** — track changes to deployment scripts, share configurations across machines, and collaborate using GitHub or other Git hosts
- **Visual Studio Code** — full PowerShell editing with IntelliSense, integrated terminal, and the PowerShell extension for authoring OSDeploy scripts
- Both tools are available in stable and pre-release (Insiders) editions and install via `winget`

{% hint style="info" %}
`-DownloadOnly` is not supported for `git`, `code`, or `code-insiders` because they are installed via `winget`, which manages its own download process. Use `-Force` to install.
{% endhint %}

## In This Section

- [Git for Windows](git.md) — Install Git for Windows and configure global identity
- [Visual Studio Code](vscode.md) — Install Visual Studio Code (stable or Insiders) for script authoring

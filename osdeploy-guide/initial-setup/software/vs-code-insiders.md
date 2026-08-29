---
description: Install Visual Studio Code Insiders with Install-OSDeploySoftware.
---

# Visual Studio Code Insiders

Visual Studio Code Insiders provides daily prerelease builds of Visual Studio Code. `Install-OSDeploySoftware` installs the `Microsoft.VisualStudioCode.Insiders` WinGet package when the `code-insiders` command is not already available.

{% hint style="info" %}
Visual Studio Code Insiders is optional. It is not required to run OSDeploy or OSDCloud.
{% endhint %}

## Preview

Review the package source and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'code-insiders'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name 'code-insiders' -Force
```

The silent installation adds file and folder context-menu commands, associates supported files, and adds the `code-insiders` command to `PATH`. It does not start Visual Studio Code Insiders automatically.

Verify the installation:

```powershell
code-insiders --version
```

{% hint style="info" %}
Visual Studio Code Insiders is installed directly through WinGet. This component does not add files to OSDeployCore and does not support `-DownloadOnly`.
{% endhint %}

## Related

* [Install Software](./)
* [Visual Studio Code reference](../../../core-components/developer-tools/vscode.md)

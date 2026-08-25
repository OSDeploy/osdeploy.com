---
description: Install the stable release of Visual Studio Code with Install-OSDeploySoftware.
---

# Visual Studio Code

Visual Studio Code provides the editor used to work with OSDeploy PowerShell scripts and repositories. `Install-OSDeploySoftware` installs the stable `Microsoft.VisualStudioCode` WinGet package when the `code` command is not already available.

{% hint style="info" %}
Visual Studio Code is optional. It is not required to run OSDeploy or OSDCloud.
{% endhint %}

## Preview

Review the package source and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'code'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name 'code' -Force
```

The silent installation adds file and folder context-menu commands, associates supported files, and adds the `code` command to `PATH`. It does not start Visual Studio Code automatically.

Verify the installation:

```powershell
code --version
```

{% hint style="info" %}
Visual Studio Code is installed directly through WinGet. This component does not add files to OSDeployCore and does not support `-DownloadOnly`.
{% endhint %}

## Related

* [Install Software](README.md)
* [Visual Studio Code reference](../../core-components/developer-tools/vscode.md)

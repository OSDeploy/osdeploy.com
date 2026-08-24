---
description: Install Git for Windows with Install-OSDeploySoftware.
---

# Git for Windows

Git for Windows provides the source-control tools used to clone and maintain OSDeploy repositories. `Install-OSDeploySoftware` installs the `Git.Git` WinGet package when `git` is not already available.

{% hint style="info" %}
Git for Windows is optional. Install it only when the workstation will clone or maintain Git repositories.
{% endhint %}

## Preview

Review the package source and install command without making changes:

```powershell
Install-OSDeploySoftware -Name 'git'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name 'git' -Force
```

After installation, the function checks the global Git identity. It prompts for `user.email` and `user.name` when they are missing and offers to change existing values.

Verify the installation and identity:

```powershell
git --version
git config --global --get-regexp '^user\.'
```

{% hint style="info" %}
Git is installed directly through WinGet. This component does not add files to OSDeployCore and does not support `-DownloadOnly`.
{% endhint %}

## Related

* [Install Software](README.md)
* [Git for Windows reference](../../core-components/developer-tools/git.md)

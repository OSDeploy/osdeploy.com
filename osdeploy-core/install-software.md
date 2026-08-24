---
description: Preview, download, and install the software used by an OSDeploy Core workstation.
---

# Install Software

{% hint style="info" %}
**TLDR:** Run `Install-OSDeploySoftware` to list available software. Specify `-Name` to preview a component, add `-Force` to install it, or add `-DownloadOnly` to download supported installers without installing them.

```powershell
Install-OSDeploySoftware
Install-OSDeploySoftware -Name 'adk-25h2'
Install-OSDeploySoftware -Name 'adk-25h2' -Force
Install-OSDeploySoftware -Name 'adk-25h2' -DownloadOnly
```
{% endhint %}

Use `Install-OSDeploySoftware` to prepare an OSDeploy Core workstation. The command can list supported components, preview an operation, download supported installers for later use, or install one or more components.

{% hint style="info" %}
Run this command from an elevated PowerShell 7 session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`. These checks run for list and preview operations as well as installations.
{% endhint %}

## Supported Software

| Name | Software | Method | Download only |
| --- | --- | --- | --- |
| `adk-25h2` | Windows ADK 10.1.26100.2454 and WinPE add-on | `curl.exe` and vendor setup | Yes |
| `mdt` | Microsoft Deployment Toolkit 6.3.8456.1000 | Verified MSI | Yes |
| `git` | Git for Windows | WinGet | No |
| `code` | Visual Studio Code | WinGet | No |
| `code-insiders` | Visual Studio Code Insiders | WinGet | No |
| `hyperv` | Hyper-V | Windows optional feature | No |
| `7zip` | 7-Zip and the 7-Zip WinPE files | WinGet and GitHub | Yes |

{% hint style="warning" %}
Microsoft has retired MDT. Install it only for an existing workflow that depends on it. See the [MDT retirement notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement).
{% endhint %}

## List Available Software

Run the command without `-Name` to list every supported component and its preview command. This operation does not install software.

```powershell
Install-OSDeploySoftware
```

## Preview an Installation

Specify a component without `-Force` to return its source, documentation, details, and install command. Use this preview to review the operation before making changes.

```powershell
Install-OSDeploySoftware -Name 'adk-25h2'
```

Preview several components in one command:

```powershell
Install-OSDeploySoftware -Name 'git', 'code', '7zip'
```

## Install Software

Add `-Force` to perform the installation. Components are processed in the order provided.

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -Force
```

Install several components in sequence:

```powershell
Install-OSDeploySoftware -Name 'git', 'code', 'code-insiders', '7zip' -Force
```

{% hint style="info" %}
Installing Git may prompt for the global `user.email` and `user.name` values. Hyper-V is skipped when OSDeploy detects that the workstation is a virtual machine. A restart may be required after enabling Hyper-V.
{% endhint %}

## Download Without Installing

Use `-DownloadOnly` with an ADK release, MDT, or 7-Zip to populate the OSDeploy Core software cache without installing the component.

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -DownloadOnly
```

```powershell
Install-OSDeploySoftware -Name 'mdt', '7zip' -DownloadOnly
```

Downloaded software is stored below:

```text
C:\ProgramData\OSDeployCore\software\
```

The 7-Zip operation also populates the versioned WinPE application cache below:

```text
C:\ProgramData\OSDeployCore\cache\winpe-apps\7zip\
```

`-DownloadOnly` is not supported for `git`, `code`, `code-insiders`, or `hyperv`. The command returns a `NotSupported` result for those components without changing the system.

## Review Results

List and preview operations return objects that can be filtered, formatted, or exported like other PowerShell output.

```powershell
Install-OSDeploySoftware -Name 'adk-25h2', '7zip' |
	Format-Table Name, Component, Source, Command -AutoSize
```

Install and download operations return a status object for each requested component. Use `-Verbose` for additional diagnostic output.

## Related

- [Install-OSDeploySoftware command reference](../powershell-modules/osdeploy/Install-OSDeploySoftware.md)
- [System Requirements](../workstation-prerequisites/system-requirements.md)
- [Install Windows ADK 25H2](../core-components/microsoft-windows-adk/install-25h2.md)
- [Install Microsoft Deployment Toolkit](../core-components/microsoft-deployment-toolkit/install-mdt.md)
- [Git for Windows](../core-components/developer-tools/git.md)
- [Visual Studio Code](../core-components/developer-tools/vscode.md)
- [Hyper-V](../core-components/windows-components/hyper-v.md)
- [7-Zip](../core-components/utilities/7zip.md)

# Import-OSDeployOS

Imports Windows OS images from mounted Windows installation media to OSDeployCore.

| Property | Value                                                                                   |
|----------|-----------------------------------------------------------------------------------------|
| Module   | OSDeploy                                                                                |
| Platform | Windows 10 or later (amd64 / arm64)                                                    |
| Requires | PowerShell 7.6, Run as Administrator, a mounted Windows installation ISO                |
| Output   | `System.IO.DirectoryInfo`                                                               |

## Description

Extracts and imports Windows OS and WinRE images from mounted Windows installation media (ISO). The function:

1. Validates administrator privileges
2. Scans for mounted Windows installation media ISO files
3. Displays an Out-GridView selection dialog for available installation indexes
4. Exports the selected Windows image (`install.wim`) and all media files
5. Extracts WinRE, WinPE, and WinSE images with metadata
6. Backs up registry hives, boot files, and OS system files
7. Creates a parallel `windows-re` directory with WinRE-specific content

Imported images are stored under `$env:ProgramData\OSDeployCore` with a naming convention of `build.revision-architecture-editionid-language` (for example, `26200.8037-amd64-enterprise-en-us`). Duplicate imports are detected and skipped automatically.

Supports both amd64 and arm64 Windows 11 installation media.

{% hint style="info" %}
Mount the Windows installation ISO via File Explorer before running this function. The ISO must be mounted and visible as a drive letter.
{% endhint %}

## Syntax

```powershell
Import-OSDeployOS [-WhatIf] [-Confirm]
```

## Parameters

This function has no named parameters. All selections are made via interactive Out-GridView dialogs.

## Examples

```powershell
# Scan for mounted Windows ISOs and import via interactive selection
Import-OSDeployOS
```

```powershell
# Import with detailed verbose output showing each extraction step
Import-OSDeployOS -Verbose
```

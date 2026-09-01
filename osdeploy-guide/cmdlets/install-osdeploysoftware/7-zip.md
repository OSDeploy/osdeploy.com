---
description: >-
  Install 7-Zip and prepare the amd64 and arm64 files used by OSDeploy boot
  images.
---

# 7-Zip

Use the exact name `7zip` to install the `7zip.7zip` WinGet package and prepare the 7-Zip files used in OSDeploy boot images.

## Requirements

Run the command from an elevated PowerShell 7.6 or later MSI installation on Windows 11 25H2 build 26200 or later.

The command requires the current OSDeploy module, `curl.exe`, `winget.exe`, and internet access. WinGet downloads the host installers; `curl.exe` downloads the WinPE app files when they are not cached.

## Preview

Return the package ID, documentation links, and install command without downloading or installing content:

```powershell
Install-OSDeploySoftware -Name '7zip'
```

## Install

Download the installers, install 7-Zip on the workstation when needed, and prepare the WinPE cache:

```powershell
Install-OSDeploySoftware -Name '7zip' -Force
```

The action performs these operations:

1. Uses WinGet to download x64 and arm64 installers under `C:\ProgramData\OSDeployCore\software\7zip.7zip\amd64` and `arm64`.
2. Installs 7-Zip silently when `C:\Program Files\7-Zip\7z.exe` is absent.
3. Downloads `7zr.exe` and the 7-Zip Extra archive for the module's current WinPE app version, then extracts the archive under `C:\ProgramData\OSDeployCore\cache\winpe-apps\7zip\26.00`.

An architecture installer folder containing any file is treated as cached. The current WinPE app cache is reused, and files or version directories from older WinPE cache versions are removed. A nonzero WinGet download exit code produces a warning for that architecture; a failed host installation stops the command.

## Download Only

Prepare both architecture installers and the WinPE app cache without installing 7-Zip on the workstation:

```powershell
Install-OSDeploySoftware -Name '7zip' -DownloadOnly
```

## WhatIf and Result

Add `-WhatIf` to `-Force` or `-DownloadOnly` to report the parent action without invoking the 7-Zip helper. No installers or cache files are created in that mode.

A completed `-Force` action returns `Component` set to `7-Zip` and `Status` set to `Installed`. A completed `-DownloadOnly` action sets `Status` to `Downloaded`. The parent result does not indicate whether the host application was newly installed or already present. The installer does not request a restart.

## Verify

Confirm the host application and both OSDeploy caches:

```powershell
& "$env:ProgramFiles\7-Zip\7z.exe" i | Select-Object -First 2
Get-ChildItem 'C:\ProgramData\OSDeployCore\software\7zip.7zip' -Directory
Get-ChildItem 'C:\ProgramData\OSDeployCore\cache\winpe-apps\7zip\26.00'
```

## Next Step

Continue with [Build-OSDeployBoot](../build-osdeployboot.md) after installing the remaining workstation requirements. See the [7-Zip reference](/broken/pages/igYUxyErBDjOY7Kl365N) for its role in OSDeploy.

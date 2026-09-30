---
description: Add an architecture-specific PowerShell 7 runtime to WinPE with Build-OSDeployBoot.
---

# OSDeploy Boot: add PowerShell 7 to WinPE

Use the `pwsh` build option to add PowerShell 7 to an OSDeploy WinPE image. `Build-OSDeployBoot` downloads the runtime that matches the boot-image architecture, caches the ZIP file, expands it into the mounted image, and configures the WinPE environment so `pwsh.exe` can be started by name.

## Requirements

Prepare the OSDeploy workstation before starting:

* Run PowerShell 7.6 or later as an administrator.
* Install the current OSDeploy and OSDCloud modules.
* Install the Windows ADK and matching WinPE add-on.
* Update OSDeploy Core content.
* Allow internet access to GitHub for the first PowerShell runtime download.

PowerShell 7 can be added to `amd64` and `arm64` boot images.

## Build WinPE with PowerShell 7

Start the build and include the `pwsh` option:

```powershell
Build-OSDeployBoot -Options 'pwsh'
```

Select the WinRE source and shared content when prompted, then approve the build.

To build directly from the AMD64 Windows ADK WinPE image:

```powershell
Build-OSDeployBoot `
    -Architecture 'amd64' `
    -UseAdkWinPE `
    -Options 'pwsh'
```

Use `arm64` instead of `amd64` when creating an ARM64 image.

## What the Build Adds

OSDeploy selects the archive for the boot-image architecture:

| Architecture | PowerShell archive |
| --- | --- |
| `amd64` | `PowerShell-7.6.4-win-x64.zip` |
| `arm64` | `PowerShell-7.6.4-win-arm64.zip` |

The download is retained in:

```text
C:\ProgramData\OSDeployCore\cache\winpe-apps\microsoft-powershell7
```

OSDeploy expands the runtime into the mounted image at:

```text
X:\Program Files\PowerShell\7
```

The build also updates the offline WinPE `SYSTEM` registry hive:

* Adds `%ProgramFiles%\PowerShell\7` to `Path`.
* Adds PowerShell 7 and user module locations to `PSModulePath`.
* Records `PowerShell 7.6.4` in the build's installed-app list.

The cached ZIP is reused during later builds. Delete the cached archive only when a clean download is required.

## Add PowerShell 7 and DaRT Together

Pass both accepted options to add PowerShell 7 and Microsoft DaRT in one AMD64 build:

```powershell
Build-OSDeployBoot -Options 'pwsh', 'dart'
```

Microsoft DaRT is not supported in ARM64 boot images. See [OSDeploy Boot: Add Microsoft DaRT to WinPE](osdeploy-boot-add-microsoft-dart-to-winpe.md) for its requirements.

## Verify PowerShell 7 in WinPE

Boot the generated ISO and run:

```powershell
Get-Command pwsh.exe
pwsh.exe -NoLogo -Command '$PSVersionTable'
```

Confirm that:

* `pwsh.exe` resolves below `X:\Program Files\PowerShell\7`.
* `PSEdition` is `Core`.
* `PSVersion` is `7.6.4`.
* `PROCESSOR_ARCHITECTURE` matches the intended boot-image architecture.

If `pwsh.exe` is missing, review the build output for the PowerShell download and archive expansion messages. Confirm that the cached ZIP exists and that the build completed without an image-servicing failure.

See [Build-OSDeployBoot](../../guide/cmdlets/build-osdeployboot.md) for all build parameters and source-selection behavior.

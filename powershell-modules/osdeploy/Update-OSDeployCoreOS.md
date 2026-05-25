# Update-OSDeployCoreOS

Imports Windows OS images from cached Enterprise ESD files to OSDeployCore.

| Property | Value                                                   |
|----------|---------------------------------------------------------|
| Module   | OSDeploy                                                |
| Platform | Windows 11 (amd64 / arm64)                             |
| Requires | PowerShell 7.6, Run as Administrator, internet access   |

## Description

Builds a complete Windows OS image layout from the Enterprise ESD files downloaded by `Update-OSDeployCoreESD`. The function first calls `Update-OSDeployCoreESD` to ensure the latest ESD files are present, then for each available architecture (amd64 and ARM64):

1. Expands ESD Index 1 (Windows Setup Media) to create the `WinOS-Media` layout
2. Exports ESD Index 2 (WinPE) to `.wim\winpe.wim`
3. Exports ESD Index 3 (WinSetup) to `.wim\winse.wim`
4. Constructs `WinOS-Media\sources\boot.wim` from the WinPE and WinSetup images
5. Exports the Enterprise image to `WinOS-Media\sources\install.wim`
6. Mounts `install.wim` to extract WinRE, registry hives, boot files, OS system files, and Ethernet/Wi-Fi drivers
7. Creates a parallel `windows-re` directory with WinRE-specific content

Imported images are stored under `$env:ProgramData\OSDeployCore` with a naming convention of `build.revision-architecture-editionid-language` (for example, `26200.8457-amd64-enterprise-en-us`). Duplicate imports are detected and skipped.

## Syntax

```powershell
Update-OSDeployCoreOS [-Force] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter  | Type     | Required | Description                                                                                                                              |
|------------|----------|----------|------------------------------------------------------------------------------------------------------------------------------------------|
| `-Force`   | `Switch` | No       | Re-downloads ESD files via `Update-OSDeployCoreESD` and re-imports all architectures even when the destination directories already exist. |
| `-WhatIf`  | `Switch` | No       | Shows which OS image directories would be created without performing any work.                                                           |
| `-Confirm` | `Switch` | No       | Prompts for confirmation before running.                                                                                                 |

## Examples

```powershell
# Ensure ESD files are cached, then import all available architectures
Update-OSDeployCoreOS
```

```powershell
# Import with detailed verbose output showing each extraction step
Update-OSDeployCoreOS -Verbose
```

```powershell
# Re-download ESD files and re-import all architectures
Update-OSDeployCoreOS -Force
```

```powershell
# Preview which OS image directories would be created
Update-OSDeployCoreOS -WhatIf
```

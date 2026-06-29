# Update-OSDeployCoreOS

Imports Windows OS images from cached Enterprise ESD files to OSDeployCore.

| Property | Value                                                   |
|----------|---------------------------------------------------------|
| Module   | OSDeploy                                                |
| Platform | Windows 11 (amd64 / arm64)                             |
| Requires | PowerShell 7.6, Run as Administrator, internet access   |

## Description

Builds a complete Windows OS image layout from the Enterprise ESD files downloaded by `Update-OSDeployCoreESD`. For each available architecture (amd64 and ARM64), or for the specified architecture when `-Architecture` is provided, the function:

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
Update-OSDeployCoreOS [[-Architecture] <String>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter        | Type     | Required | Description                                                                                          |
|------------------|----------|----------|------------------------------------------------------------------------------------------------------|
| `-Architecture`  | `String` | No       | Limits processing to a single architecture: `amd64` or `arm64`. When omitted, all available ESD files are imported. |
| `-WhatIf`        | `Switch` | No       | Shows which OS image directories would be created without performing any work.                       |
| `-Confirm`       | `Switch` | No       | Prompts for confirmation before running.                                                             |

## Examples

```powershell
# Import all available OS image architectures from cached ESD files
Update-OSDeployCoreOS
```

```powershell
# Import with detailed verbose output showing each extraction step
Update-OSDeployCoreOS -Verbose
```

```powershell
# Import only amd64 OS images
Update-OSDeployCoreOS -Architecture amd64
```

```powershell
# Preview which OS image directories would be created
Update-OSDeployCoreOS -WhatIf
```

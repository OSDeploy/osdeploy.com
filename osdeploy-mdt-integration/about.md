# About

PowerShell module for automating MDT (Microsoft Deployment Toolkit) deployments with OSDCloud support.

|                    |                                                                       |
| ------------------ | --------------------------------------------------------------------- |
| **Author**         | David Segura                                                          |
| **Company**        | Recast Software                                                       |
| **License**        | See [LICENSE](/broken/pages/e79ca4df3b17a940d48033651080ff3ed16cfe1e) |
| **PS Editions**    | Core (7+), Desktop (5.1)                                              |
| **Min PS Version** | 5.1                                                                   |

***

## Requirements

* **PowerShell 7+** — required for `Enable-OSDeployMDT` and `Build-OSDeployMDT`
* **Windows PowerShell 5.1** — loads MDT cmdlet helpers only; no public functions exported
* Windows ADK installed (for `Build-OSDeployMDT` WinPE customization)
* MDT installed (for MDT cmdlet support in PS5.1)

***

## Installation

### From the PowerShell Gallery _(future)_

```powershell
Install-Module -Name OSDeployMDT
```

### From Source

```powershell
git clone https://github.com/OSDeploy/OSDeployMDT.git
Import-Module .\OSDeployMDT\OSDeployMDT.psd1
```

***

## Functions

### `Enable-OSDeployMDT`

One-time initialization of an MDT Deployment Share for OSDCloud integration. Run this once after creating a new deployment share, before the first **Update Deployment Share**.

* Creates `Templates\`, `Templates\winpe-driver\`, and `Templates\winpe-extrafiles\` folders
* Copies `LiteTouchPE.xml`, `winpeshl.ini`, `wimscript.ini`, `Unattend_PE_x64.xml` from `INSTALLDIR\Templates`
* Copies `Background.bmp` wallpaper from `INSTALLDIR\Samples` (replaceable)
* Rewrites `%INSTALLDIR%` path references in `LiteTouchPE.xml` to `%DEPLOYROOT%`
* Syncs WinPE `<Components>` from `Boot\LiteTouchPE_x64.xml` (or writes a default set)
* Registers `Build-OSDeployMDT` as a LiteTouchPE exit so it runs on every Update Deployment Share
* Updates `Control\Settings.xml`: disables x86 boot image, sets scratch space to 512 MB, sets paths

```powershell
Enable-OSDeployMDT
```

> Requires Administrator rights. See [docs/Enable-OSDeployMDT.md](/broken/pages/6b8c26e7b08663758456ff9d131d1686d3632c85) for full details.

***

### `Build-OSDeployMDT`

MDT LiteTouchPE exit script — runs automatically during **Update Deployment Share**. Requires PowerShell 7+.

**WIM stage** — customizes the mounted WinPE image:

* Collects EFI boot files and ADK `oscdimg` binaries into `DEPLOYROOT\Boot\bootbins\`
* Copies `oa3tool.exe` (OEM Activation 3.0) into WinPE `System32`
* Applies DISM locale and time-zone settings
* Adds `PackageManagement` and `PowerShellGet` modules (enables PowerShell Gallery in WinPE)
* Adds `AzCopy`, `curl`, and `7-Zip` to `System32`
* Saves the `OSDCloud` module into the WinPE image
* Injects WinPE drivers from `DEPLOYROOT\Templates\winpe-driver\` (automatic) and optionally from the OSDeployCore library (interactive picker)

**POSTISO stage** — builds a patched ISO with `bootmgfw_EX.efi` for UEFI CA 2023 Secure Boot compliance.

```powershell
# Default — uses current system locale and time zone
Build-OSDeployMDT

# French locale
Build-OSDeployMDT -SetAllIntl 'fr-fr' -SetTimeZone 'Romance Standard Time'
```

| Parameter         | Type   | Description                                                                              |
| ----------------- | ------ | ---------------------------------------------------------------------------------------- |
| `-SetAllIntl`     | String | IETF locale tag for all International settings. Default: `en-us`.                        |
| `-SetInputLocale` | String | Default keyboard input locale. Default: `en-us`.                                         |
| `-SetTimeZone`    | String | WinPE time zone name (validated against `tzutil /l`). Default: current system time zone. |

> See [docs/Build-OSDeployMDT.md](/broken/pages/6a627d21ea4b8d13db94d47421595226c4519917) for full details.

***

## Typical Workflow

```powershell
# 1. Create an MDT Deployment Share in the MDT Workbench (one-time)

# 2. Initialize the share for OSDCloud (one-time, run as Administrator in PS7)
Enable-OSDeployMDT

# 3. Run "Update Deployment Share" in the MDT Workbench
#    Build-OSDeployMDT runs automatically and customizes the WinPE boot image

# 4. Boot a device from the WinPE image and run:
Deploy-OSDCloud
```

***

## Project Structure

```
OSDeployMDT/
├── OSDeployMDT/
│   ├── module.json                               # URLs, versions, SHA256 checksums for downloads
│   ├── OSDeployMDT.psd1                          # Module manifest
│   ├── OSDeployMDT.psm1                          # Barrel file — edition-split loader
│   ├── Public/                                   # Exported functions (PS7/Core only)
│   │   ├── Enable-OSDeployMDT.ps1
│   │   └── Build-OSDeployMDT.ps1
│   └── Private/                                  # Internal helpers (not exported)
│       ├── Get-MDTDeploymentShare.ps1
│       ├── Get-MDTDeploymentToolsXmlPath.ps1
│       ├── Get-MDTEnvINSTALLDIR.ps1
│       ├── Get-OSDeployMDTPath.ps1
│       ├── Get-WindowsAdkPaths.ps1
│       ├── Import-MDTCmdlets.ps1
│       ├── Initialize-OSDeployMDTModule.ps1
│       ├── Install-MicrosoftDeploymentToolkit.ps1
│       ├── Install-MicrosoftWindowsAdk25H2.ps1
│       ├── Select-MDTDeploymentShare.ps1
│       └── Steps/
│           ├── Step-BuildMediaAddWallpaper.ps1
│           ├── Step-BuildMediaDismSettings.ps1
│           ├── Step-BuildMediaPowerShellUpdate.ps1
│           ├── Step-WinPEAppAzCopy.ps1
│           ├── Step-WinPEAppCurl.ps1
│           └── Step-WinPEAppZip.ps1
└── tests/
    └── Public/
        └── Test-OSDeployMDTLicense.Tests.ps1
```

***

## Running Tests

[Pester v5](https://pester.dev/) is required.

```powershell
# Install Pester if needed
Install-Module Pester -Force -SkipPublisherCheck

# Run all tests from the repo root
Invoke-Pester -Path .\tests\ -Output Detailed
```

***

## Contributing

1. Fork the repository and create a feature branch.
2. Add or update the relevant function in `OSDeployMDT\Public\` or `OSDeployMDT\Private\`.
3. Add a corresponding `*.Tests.ps1` file under `tests\`.
4. Update `FunctionsToExport` in `OSDeployMDT.psd1` for any new public functions.
5. Bump `ModuleVersion` in `OSDeployMDT.psd1` following [Semantic Versioning](https://semver.org/).
6. Add an entry to [CHANGELOG.md](/broken/pages/117e9ff0bd21b4a37e6e644747afe2684a30a6cf).
7. Open a Pull Request.

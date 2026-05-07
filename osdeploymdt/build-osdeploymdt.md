# Build-OSDeployMDT

## Purpose

`Build-OSDeployMDT` is an **MDT LiteTouchPE exit script** that runs automatically during **Update Deployment Share**. It is invoked by MDT at each stage of the boot image build process and performs all WinPE customizations needed to produce an OSDCloud-ready boot image.

It is registered as an exit in `DEPLOYROOT\Templates\LiteTouchPE.xml` by `Enable-OSDeployMDT`:

```xml
<Exit>start /wait pwsh.exe -ExecutionPolicy Bypass -Command "Build-OSDeployMDT"</Exit>
```

> Called by MDT automatically. Can also be run interactively with no arguments to display help.

***

## Parameters

| Parameter        | Type     | Default                  | Description                                              |
| ---------------- | -------- | ------------------------ | -------------------------------------------------------- |
| `SetAllIntl`     | `string` | `en-us`                  | Sets all WinPE International locale settings             |
| `SetInputLocale` | `string` | `en-us`                  | Sets the default WinPE input locale                      |
| `SetTimeZone`    | `string` | Current system time zone | Sets the WinPE time zone (validated against `tzutil /l`) |

***

## MDT Environment Variables Consumed

MDT injects these environment variables when calling the exit script:

| Variable       | Description                                        |
| -------------- | -------------------------------------------------- |
| `STAGE`        | Build stage: `WIM`, `POSTWIM`, `ISO`, or `POSTISO` |
| `CONTENT`      | Path relevant to the current stage (see below)     |
| `ARCHITECTURE` | `amd64` or `x86`                                   |
| `ADKPath`      | Windows ADK installation path                      |
| `INSTALLDIR`   | MDT installation directory                         |
| `DEPLOYROOT`   | MDT deployment share root                          |
| `TEMPLATE`     | MDT template name (`LiteTouchPE` or `Generic`)     |

### CONTENT by Stage

| STAGE     | CONTENT value                                |
| --------- | -------------------------------------------- |
| `WIM`     | Path to the **mounted WIM** (temp directory) |
| `POSTWIM` | Path to the captured `.wim` file             |
| `ISO`     | Path to the ISO staging directory            |
| `POSTISO` | Path to the captured `.iso` file             |

***

## What It Does

### When `STAGE = WIM` (primary stage)

This is the main customization stage. `CONTENT` points to the mounted WinPE image root.

#### 1. Save EFI Boot Files (bootbins)

Creates `DEPLOYROOT\Boot\bootbins\` and copies the following files for later use:

* `bootmgfw.efi` and `bootmgfw_EX.efi` from the mounted WIM
* `efisys.bin`, `efisys_noprompt.bin`, `efisys_EX.bin`, `efisys_noprompt_EX.bin`, `etfsboot.com` from the ADK `oscdimg` directory

#### 2. Copy OA3Tool

Copies `oa3tool.exe` from the Windows ADK into `MountPath\Windows\System32\` to enable OEM Activation 3.0 support in WinPE.

#### 3. Apply DISM Settings

Calls `Step-BuildMediaDismSettings` to configure WinPE locale and time zone using the `SetAllIntl`, `SetInputLocale`, and `SetTimeZone` parameters.

#### 4. Update PowerShell Package Management

Calls `Step-BuildMediaPowerShellUpdate` to add `PackageManagement` and `PowerShellGet` modules to the WinPE image, enabling **PowerShell Gallery** access at boot time.

#### 5. Add WinPE Applications

| Step                  | What it adds                              |
| --------------------- | ----------------------------------------- |
| `Step-WinPEAppAzCopy` | AzCopy — Azure Blob Storage transfer tool |
| `Step-WinPEAppCurl`   | curl — HTTP client                        |
| `Step-WinPEAppZip`    | 7-Zip — archive tool                      |

All binaries are placed in WinPE `System32` for direct command-line access.

#### 6. Save OSDCloud Module

Downloads and saves the `OSDCloud` PowerShell module directly into the WinPE image:

```powershell
Save-Module -Name OSDCloud -Path "$MountPath\Program Files\WindowsPowerShell\Modules"
```

This enables `Deploy-OSDCloud` to run inside WinPE without internet access to the PowerShell Gallery.

#### 7. WinPE Driver Injection

Drivers are tracked in a JSON log file (`winpe-driver.json`) to avoid re-injecting drivers on subsequent builds.

**Automatic — from Deployment Share:**

All sub-folders found in `DEPLOYROOT\Templates\winpe-driver\` are injected using `Add-WindowsDriver -ForceUnsigned -Recurse`. Previously applied drivers are skipped.

**Interactive — from OSDeployCore library:**

If drivers exist under `%ProgramData%\OSDeployCore\library\*\winpe-driver\amd64\*`, an `Out-GridView` picker is shown. Selected drivers are injected and recorded in the log.

The driver log is written to both the mounted WIM (`MountPath\winpe-driver.json`) and `DEPLOYROOT\Boot\winpe-driver.json` after injection.

***

### When `STAGE = POSTWIM`

No actions are currently implemented.

***

### When `STAGE = ISO`

No actions are currently implemented.

***

### When `STAGE = POSTISO`

Builds a **patched ISO** with an updated EFI boot image to support Secure Boot Extended (`_EX`) variants (e.g., `bootmgfw_EX.efi`).

#### Steps:

1. **Copy `bootmgfw_EX.efi`** from `DEPLOYROOT\Boot\bootbins\` into the ISO staging folder at `EFI\MICROSOFT\BOOT\bootmgfw.efi`.
2.  **Build patched ISO** using `oscdimg.exe` from the Windows ADK, producing:

    ```
    DEPLOYROOT\Boot\<IsoBaseName>_ca2023.iso
    ```

    The ISO is built with dual-boot support (`-bootdata:2`) using `etfsboot.com` (BIOS) and `efisys_EX.bin` (UEFI Secure Boot Extended).

***

## Output

| Artifact         | Location                            |
| ---------------- | ----------------------------------- |
| Bootbins         | `DEPLOYROOT\Boot\bootbins\`         |
| WinPE driver log | `DEPLOYROOT\Boot\winpe-driver.json` |
| Patched ISO      | `DEPLOYROOT\Boot\<name>_ca2023.iso` |

***

## Examples

```powershell
# Run automatically by MDT (STAGE and CONTENT set by MDT environment)
Build-OSDeployMDT

# Run with French locale and European time zone
Build-OSDeployMDT -SetAllIntl 'fr-fr' -SetTimeZone 'Romance Standard Time'

# Run interactively with no MDT environment → displays help
Build-OSDeployMDT
```

***

## Relationship to Enable-OSDeployMDT

| Function             | When to run                                    | What it does                                               |
| -------------------- | ---------------------------------------------- | ---------------------------------------------------------- |
| `Enable-OSDeployMDT` | Once, after creating a new deployment share    | Configures the share (templates, settings, registers exit) |
| `Build-OSDeployMDT`  | Automatically on every Update Deployment Share | Customizes the WinPE image during the build                |

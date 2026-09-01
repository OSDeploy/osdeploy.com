# About

OSDeploy integrates with MDT through two public functions:

* `Install-OSDeployMDT` initializes an existing MDT Deployment Share one time.
* `Invoke-OSDeployMDT` runs as the LiteTouchPE exit script during every Update Deployment Share operation.

***

## Requirements

* PowerShell 7+ on the MDT host
* MDT installed on the same machine
* Windows ADK installed for WinPE customization and ISO patching
* Administrator rights

***

## Installation

Install the OSDeploy module from the PowerShell Gallery:

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
```

***

## Functions

### `Install-OSDeployMDT`

Initializes an MDT Deployment Share for use with OSDeploy and configures it so that `Invoke-OSDeployMDT` runs automatically on every **Update Deployment Share**.

Without `-Force`, the function runs in audit mode and reports the current state without changing anything.

With `-Force`, it performs the following work:

* Resolves the MDT installation directory and active deployment share
* Creates `Templates\`, `Templates\winpe-drivers\`, and `Templates\winpe-extrafiles\`
* Copies `winpeshl.ini`, `Wimscript.ini`, `Unattend_PE_x64.xml`, and `LiteTouchPE.xml`
* Copies `Background.bmp` to `Templates\background.bmp`
* Rewrites `%INSTALLDIR%` references in `LiteTouchPE.xml` to `%DEPLOYROOT%`
* Copies the WinPE `<Components>` block from `Boot\LiteTouchPE_x64.xml` or writes a default set
* Registers `Invoke-OSDeployMDT` as the LiteTouchPE exit command
* Updates `Control\Settings.xml` to disable x86 boot image generation and set scratch space to 512 MB

```powershell
Install-OSDeployMDT -Force
```

See [powershell-modules/osdeploy/Install-OSDeployMDT.md](../../command-reference/osdeploy/install-osdeploymdt.md) for the full function reference.

***

### `Invoke-OSDeployMDT`

MDT LiteTouchPE exit script that customizes the WinPE image during **Update Deployment Share**.

At the `WIM` stage it:

* Collects EFI boot files and ADK `oscdimg` binaries into `Boot\bootbins\`
* Copies `oa3tool.exe` into WinPE `System32`
* Applies DISM locale and time-zone settings
* Adds `PackageManagement` and `PowerShellGet` to WinPE
* Adds `AzCopy`, `curl`, and `7-Zip` tools to WinPE `System32`
* Saves the `OSDCloud` module into the mounted WinPE image
* Injects WinPE drivers from `Templates\winpe-drivers\` and, when available, the OSDeployCore driver library

At the `POSTISO` stage it:

* Patches the exported ISO with `bootmgfw_EX.efi` for UEFI CA 2023 Secure Boot compatibility

```powershell
Invoke-OSDeployMDT
Invoke-OSDeployMDT -SetTimeZone 'Romance Standard Time'
```

| Parameter         | Type   | Description                                                  |
| ----------------- | ------ | ------------------------------------------------------------ |
| `-SetInputLocale` | String | Sets the default WinPE input locale. Default: `en-us`.       |
| `-SetTimeZone`    | String | Sets the WinPE time zone. Default: current system time zone. |

See [powershell-modules/osdeploy/Invoke-OSDeployMDT.md](../../command-reference/osdeploy/invoke-osdeploymdt.md) for the full function reference.

***

## Typical Workflow

```powershell
# 1. Create an MDT Deployment Share in the MDT Workbench

# 2. Initialize the share for OSDeploy
Install-OSDeployMDT -Force

# 3. Run Update Deployment Share in the MDT Workbench
#    Invoke-OSDeployMDT runs automatically and customizes WinPE

# 4. Boot the WinPE image and run:
Deploy-OSDCloud
```

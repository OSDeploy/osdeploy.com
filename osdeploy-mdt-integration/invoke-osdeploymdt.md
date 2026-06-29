# Invoke-OSDeployMDT

## Purpose

`Invoke-OSDeployMDT` is the MDT LiteTouchPE exit script used by OSDeploy. MDT calls it automatically during **Update Deployment Share** so the WinPE image is customized at build time.

It is registered in `DEPLOYROOT\Templates\LiteTouchPE.xml` by `Install-OSDeployMDT`:

```xml
<Exit>start /wait pwsh.exe -ExecutionPolicy Bypass -Command "Invoke-OSDeployMDT"</Exit>
```

> Called by MDT automatically. If run manually without the MDT environment variables, it displays help and returns.

***

## Parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `SetInputLocale` | `string` | `en-us` | Sets the default WinPE input locale. |
| `SetTimeZone` | `string` | Current system time zone | Sets the WinPE time zone, validated against `tzutil /l`. |

***

## MDT Environment Variables Consumed

MDT injects these environment variables when calling the exit script:

| Variable | Description |
| --- | --- |
| `STAGE` | Build stage: `WIM`, `POSTWIM`, `ISO`, or `POSTISO` |
| `CONTENT` | Path relevant to the current stage |
| `ARCHITECTURE` | `amd64` or `x86` |
| `ADKPath` | Windows ADK installation path |
| `INSTALLDIR` | MDT installation directory |
| `DEPLOYROOT` | MDT deployment share root |
| `TEMPLATE` | MDT template name (`LiteTouchPE` or `Generic`) |

### CONTENT by stage

| STAGE | CONTENT value |
| --- | --- |
| `WIM` | Path to the mounted WIM temporary directory |
| `POSTWIM` | Path to the captured `.wim` file |
| `ISO` | Path to the ISO staging directory |
| `POSTISO` | Path to the captured `.iso` file |

***

## What It Does

### `STAGE = WIM`

This is the primary customization stage. `CONTENT` points to the mounted WinPE image root.

It performs these actions:

* Saves EFI boot files into `DEPLOYROOT\Boot\bootbins\`
* Copies `oa3tool.exe` into WinPE `System32`
* Applies DISM locale and time-zone settings
* Adds `PackageManagement` and `PowerShellGet` to WinPE
* Adds `AzCopy`, `curl`, and `7-Zip` to WinPE `System32`
* Saves the `OSDCloud` PowerShell module into the image
* Injects WinPE drivers from `DEPLOYROOT\Templates\winpe-drivers\`
* Injects additional drivers from the OSDeployCore library through an interactive picker when available

The applied driver list is written to `MountPath\winpe-drivers.json` and copied to `DEPLOYROOT\Boot\winpe-drivers.json`.

### `STAGE = POSTWIM`

No actions are currently implemented.

### `STAGE = ISO`

No actions are currently implemented.

### `STAGE = POSTISO`

Builds a patched ISO with an updated EFI boot image for Secure Boot Extended compatibility.

It copies `bootmgfw_EX.efi` into the ISO staging tree at `EFI\MICROSOFT\BOOT\bootmgfw.efi`, then builds the patched ISO with `oscdimg.exe` from the Windows ADK.

The output is written to:

```text
DEPLOYROOT\Boot\<IsoBaseName>_uefi2023ca.iso
```

***

## Output

| Artifact | Location |
| --- | --- |
| Bootbins | `DEPLOYROOT\Boot\bootbins\` |
| WinPE driver log | `DEPLOYROOT\Boot\winpe-drivers.json` |
| Patched ISO | `DEPLOYROOT\Boot\<name>_uefi2023ca.iso` |

***

## Examples

```powershell
# Run automatically by MDT when STAGE and CONTENT are set
Invoke-OSDeployMDT

# Run with a specific time zone
Invoke-OSDeployMDT -SetTimeZone 'Romance Standard Time'
```

***

## Relationship to Install-OSDeployMDT

| Function | When to run | What it does |
| --- | --- | --- |
| `Install-OSDeployMDT` | Once, after creating a new deployment share | Configures the share, registers the exit command, and updates settings |
| `Invoke-OSDeployMDT` | Automatically on every Update Deployment Share | Customizes the WinPE image during the build |
# Install-OSDeployMDT

## Purpose

`Install-OSDeployMDT` initializes an existing MDT Deployment Share for OSDeploy. It is a one-time setup function that configures the share so that `Invoke-OSDeployMDT` runs automatically during **Update Deployment Share** and WinPE is customized with the required settings, tools, and drivers.

> Requires local **Administrator** rights.

***

## What It Does

### Audit mode

Without `-Force`, the function runs in audit mode and reports the current deployment share state without making changes.

### Initialization steps

With `-Force`, the function performs these actions:

1. Resolves the MDT installation directory (`INSTALLDIR`) and active deployment share (`DEPLOYROOT`).
2. Creates `Templates\`, `Templates\winpe-drivers\`, and `Templates\winpe-extrafiles\`.
3. Copies `winpeshl.ini`, `Wimscript.ini`, `Unattend_PE_x64.xml`, and `LiteTouchPE.xml` from `INSTALLDIR\Templates`.
4. Copies `Background.bmp` from `INSTALLDIR\Samples` to `DEPLOYROOT\Templates\background.bmp`.
5. Rewrites `%INSTALLDIR%` references in `LiteTouchPE.xml` so the templates under `DEPLOYROOT` are used.
6. Copies or writes the WinPE `<Components>` block from `Boot\LiteTouchPE_x64.xml`.
7. Registers `Invoke-OSDeployMDT` as the LiteTouchPE exit command.
8. Updates `Control\Settings.xml` to disable x86 boot image generation and set x64 scratch space to 512 MB.

### Result

After a successful run, the next **Update Deployment Share** operation uses the customized WinPE templates, invokes `Invoke-OSDeployMDT`, and exposes the `winpe-drivers` and `winpe-extrafiles` drop locations in the deployment share.

***

## Example

```powershell
Install-OSDeployMDT -Force
```

Run once after creating a new MDT Deployment Share, before the first **Update Deployment Share**.
# Invoke-OSDeployMDT

MDT LiteTouchPE exit script — runs automatically on every Update Deployment Share.

| Property | Value                                                                            |
|----------|----------------------------------------------------------------------------------|
| Module   | OSDeploy                                                                         |
| Platform | Windows (MDT deployment share host)                                              |
| Requires | MDT installed, `Install-OSDeployMDT` previously run, Run as Administrator        |

## Description

Called automatically by MDT as a LiteTouchPE exit script at each stage of the **Update Deployment Share** operation. The function reads the `$Env:STAGE` environment variable to determine which actions to take.

If `$Env:STAGE` is not set (for example, when the function is run manually), it displays help and returns without making changes.

**STAGE = WIM** (mounted WinPE image available):

- Collects EFI boot files and ADK `oscdimg` binaries to `DEPLOYROOT\Boot\bootbins\`
- Copies ADK OA3Tool to WinPE `System32`
- Applies DISM locale and time-zone settings
- Adds PackageManagement and PowerShellGet modules to WinPE
- Adds AzCopy, curl, and 7-Zip (`7zr.exe`) to WinPE `System32`
- Saves the OSDCloud PowerShell module into the WinPE image
- Injects WinPE drivers via an interactive picker

**STAGE = POSTISO**:

- Builds a patched ISO with `bootmgfw_EX.efi` for UEFI CA 2023 Secure Boot compatibility

MDT environment variables consumed: `STAGE`, `CONTENT`, `ARCHITECTURE`, `INSTALLDIR`, `DEPLOYROOT`.

## Syntax

```powershell
Invoke-OSDeployMDT [-SetInputLocale <String>] [-SetTimeZone <String>]
```

## Parameters

| Parameter          | Type     | Required | Description                                                                                      |
|--------------------|----------|----------|--------------------------------------------------------------------------------------------------|
| `-SetInputLocale`  | `String` | No       | Sets the default input locale in WinPE. Default: `en-us`.                                        |
| `-SetTimeZone`     | `String` | No       | Sets the WinPE time zone. Validated against `tzutil /l`. Default: the current system time zone.  |

## Examples

```powershell
# Run exit customization using current system locale and time zone
Invoke-OSDeployMDT
```

```powershell
# Run with a specific time zone
Invoke-OSDeployMDT -SetTimeZone 'Romance Standard Time'
```

# Invoke-HPTPMEXEDownload

Downloads the required HP TPM firmware EXE to C:\OSDCloud\HP\TPM.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Calls Get-HPTPMDetermine to identify the required softpaq, then downloads the firmware
EXE to C:\OSDCloud\HP\TPM.
If the file is already present on a connected OSDCloud USB
drive it is copied locally instead of being downloaded from the internet.
The destination
folder is cleared before each run.
Also disables Virtualization Technology (VTx) in the
BIOS via Set-HPBIOSSetting.

## Syntax

```powershell
Invoke-HPTPMEXEDownload [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Invoke-HPTPMEXEDownload
Determines the required TPM softpaq and downloads (or copies) it to C:\OSDCloud\HP\TPM.
```

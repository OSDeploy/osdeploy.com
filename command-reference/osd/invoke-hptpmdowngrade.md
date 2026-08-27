# Invoke-HPTPMDowngrade

Downloads and applies the HP SP94937 softpaq to downgrade a TPM from 2.0 to 1.2.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads softpaq SP94937 using HPCMSL, extracts it, and runs TPMConfig64.exe with
the '-a 1.2' argument to downgrade an Infineon TPM from firmware version 2.0 to 1.2.
Disables Virtualization Technology (VTx) in the BIOS via Set-HPBIOSSetting before
applying the firmware change.

## Syntax

```powershell
Invoke-HPTPMDowngrade [[-WorkingFolder] <Object>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WorkingFolder` | `Object` | False | The folder path where the softpaq EXE will be downloaded and extracted. Defaults to $env:TEMP\TPM if not specified. |

## Examples

### Example
```powershell
Invoke-HPTPMDowngrade
Downloads SP94937 to $env:TEMP\TPM and downgrades the Infineon TPM to spec 1.2.
```

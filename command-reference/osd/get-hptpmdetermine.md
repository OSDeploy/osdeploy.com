# Get-HPTPMDetermine

Determines which HP TPM firmware update package is required for the current device.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Queries the TPM via WMI (win32_tpm) to identify the manufacturer and firmware version.
For Infineon (IFX) TPMs, compares the firmware version against known vulnerable version
ranges and returns the appropriate HP softpaq package ID.
Returns 'SP87753' for firmware requiring an older update package, 'SP94937' for firmware
requiring the newer package, or $false if no update is needed or the TPM is not Infineon.

## Syntax

```powershell
Get-HPTPMDetermine [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
$Package = Get-HPTPMDetermine
Returns 'SP87753', 'SP94937', or $false.
```

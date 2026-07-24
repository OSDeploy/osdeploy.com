# Invoke-HPTPMDownload

Downloads and extracts the required HP TPM firmware update softpaq using HPCMSL.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Calls Get-HPTPMDetermine to identify the required softpaq, then uses the HPCMSL
Get-Softpaq cmdlet to download it to the specified working folder.
The downloaded
EXE is silently extracted to a subfolder.
Returns the path to the extracted folder.
Intended for manual download and testing scenarios.

## Syntax

```powershell
Invoke-HPTPMDownload [[-WorkingFolder] <Object>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WorkingFolder` | `Object` | False | The folder path where the softpaq EXE will be downloaded and extracted. Defaults to $env:TEMP\TPM if not specified. |

## Examples

### Example
```powershell
Invoke-HPTPMDownload
Downloads and extracts the required TPM firmware softpaq to $env:TEMP\TPM.
```

### Example
```powershell
Invoke-HPTPMDownload -WorkingFolder 'C:\Temp\TPMWork'
Downloads and extracts the required TPM firmware softpaq to C:\Temp\TPMWork.
```

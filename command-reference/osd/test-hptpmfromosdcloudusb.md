# Test-HPTPMFromOSDCloudUSB

Tests whether HP TPM firmware packages exist on an OSDCloud USB drive.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Searches for HP TPM firmware softpaq files (SP87753 and/or SP94937) on a connected
OSDCloud USB volume.
If found, optionally copies them to C:\OSDCloud\HP for local use.
Returns $true if the requested package(s) are found, otherwise $false.

## Syntax

```powershell
Test-HPTPMFromOSDCloudUSB [[-PackageID] <String>] [-TryToCopy] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-PackageID` | `String` | False | The HP softpaq package ID to check for. Valid values are 'SP87753' or 'SP94937'. If not specified, both packages are checked. |
| `-TryToCopy` | `SwitchParameter` | False | Switch to indicate that found firmware files should be copied to C:\OSDCloud\HP. Note: this parameter is currently unreachable due to early return statements when a PackageID is specified. |

## Examples

### Example
```powershell
Test-HPTPMFromOSDCloudUSB -PackageID SP94937
Returns $true if SP94937.exe exists on the OSDCloud USB and copies it to C:\OSDCloud\HP.
```

### Example
```powershell
Test-HPTPMFromOSDCloudUSB
Returns $true only if both SP87753.exe and SP94937.exe exist on the OSDCloud USB.
```

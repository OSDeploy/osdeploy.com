# Save-WinPECloudDriver

Download and expand WinPE Drivers

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Download and expand WinPE Drivers
This function must be run in Windows

## Syntax

```powershell
Save-WinPECloudDriver [-CloudDriver <String[]>] [-DriverHWID <String[]>] [-Path <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-CloudDriver` | `String[]` | False | WinPE Driver: Download and install in WinPE drivers from Dell,HP,IntelNet,LenovoDock,Nutanix,Surface,USB,VMware,WiFi |
| `-DriverHWID` | `String[]` | False | WinPE Driver: HardwareID of the Driver download from Microsoft Catalog |
| `-Path` | `String` | False | WinPE Driver: Destination path to save the drivers If not specified, a random directory in $env:TEMP is selected |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

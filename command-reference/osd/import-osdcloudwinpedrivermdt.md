# Import-OSDCloudWinPEDriverMDT

Imports OSDCloud CloudDrivers into an MDT Deployment Share

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Imports OSDCloud CloudDrivers into an MDT Deployment Share

## Syntax

```powershell
Import-OSDCloudWinPEDriverMDT [[-Driver] <String[]>] [[-DriverHWID] <String[]>] [[-ShareName] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Driver` | `String[]` | False | WinPE Driver: Download and install in WinPE drivers from Dell,HP,IntelNet,LenovoDock,Nutanix,Surface,USB,VMware,WiFi |
| `-DriverHWID` | `String[]` | False | WinPE Driver: HardwareID of the Driver to add to WinPE |
| `-ShareName` | `String` | False | No additional description provided. |

## Examples

### Example
```powershell
Import-OSDCloudWinPEDriverMDT
Imports OSDCloud WinPE cloud drivers into an MDT deployment share.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

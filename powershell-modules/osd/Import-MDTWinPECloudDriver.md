# Import-MDTWinPECloudDriver

Imports OSDCloud CloudDrivers into an MDT Deployment Share

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Imports OSDCloud CloudDrivers into an MDT Deployment Share

## Syntax

```powershell
Import-MDTWinPECloudDriver [[-CloudDriver] <String[]>] [[-DriverHWID] <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-CloudDriver` | `String[]` | False | WinPE Driver: Download and install in WinPE drivers from Dell,HP,IntelNet,LenovoDock,Nutanix,Surface,USB,VMware,WiFi |
| `-DriverHWID` | `String[]` | False | WinPE Driver: HardwareID of the Driver to add to WinPE |

## Examples

### Example
```powershell
Import-MDTWinPECloudDriver
Imports OSDCloud WinPE cloud drivers into the configured MDT deployment share.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Edit-MyWinPE

Mounts and edits a WinPE WIM file

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Mounts and edits a WinPE WIM file

## Syntax

```powershell
Edit-MyWinPE [-ImagePath <String[]>] [-Index <UInt32>] [-CloudDriver <String[]>] [-DriverHWID <String[]>]
 [-DriverPath <String[]>] [-ExecutionPolicy <String>] [-PSModuleInstall <String[]>] [-PSModuleCopy <String[]>]
 [-PSGallery] [-Wallpaper <String>] [-DismountSave] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ImagePath` | `String[]` | False | Path to the WinPE WIM file. This file must be local and not on a USB or Network Share |
| `-Index` | `UInt32` | False | Index of the WinPE WIM file to mount. Default is 1 |
| `-CloudDriver` | `String[]` | False | WinPE Driver: Download and install in WinPE drivers from Dell,HP,IntelNet,LenovoDock,Nutanix,Surface,USB,VMware,WiFi |
| `-DriverHWID` | `String[]` | False | WinPE Driver: HardwareID of the Driver to add to WinPE |
| `-DriverPath` | `String[]` | False | WinPE Driver: Path to additional Drivers you want to add to WinPE |
| `-ExecutionPolicy` | `String` | False | PowerShell: Sets the PowerShell Execution Policy of WinPE. Bypass is recommended |
| `-PSModuleInstall` | `String[]` | False | PowerShell: Installs named PowerShell Modules from PowerShell Gallery to WinPE |
| `-PSModuleCopy` | `String[]` | False | PowerShell: Copies named PowerShell Modules from the running OS to WinPE This is useful for adding Modules that are customized or not on PowerShell Gallery |
| `-PSGallery` | `SwitchParameter` | False | PowerShell: Enables PowerShell Gallery functionality in WinPE |
| `-Wallpaper` | `String` | False | Sets the specified Wallpaper JPG file as the WinPE Background |
| `-DismountSave` | `SwitchParameter` | False | Dismounts and saves changes to the mounted WinPE WIM |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

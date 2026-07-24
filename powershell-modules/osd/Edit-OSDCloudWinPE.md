# Edit-OSDCloudWinPE

Edits content by using Edit-OSDCloudWinPE.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for Edit-OSDCloudWinPE.

## Syntax

```powershell
Edit-OSDCloudWinPE [-CloudDriver <String[]>] [-StartOSDCloudGUI] [-DriverHWID <String[]>]
 [-DriverPath <String[]>] [-PSModuleCopy <String[]>] [-PSModuleInstall <String[]>] [-Startnet <String>]
 [-StartOSDCloud <String>] [-StartOSDPad <String>] [-StartPSCommand <String>] [-StartURL <String>] [-UpdateUSB]
 [-Wallpaper <FileInfo>] [-UseDefaultWallpaper] [-Brand <String>] [-WorkspacePath <String>] [-WirelessConnect]
 [-WifiProfile <FileInfo>] [-Add7Zip] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-CloudDriver` | `String[]` | False | Specifies the value for CloudDriver. |
| `-StartOSDCloudGUI` | `SwitchParameter` | False | Indicates whether to enable StartOSDCloudGUI. |
| `-DriverHWID` | `String[]` | False | Specifies the value for DriverHWID. |
| `-DriverPath` | `String[]` | False | Specifies the value for DriverPath. |
| `-PSModuleCopy` | `String[]` | False | Specifies the value for PSModuleCopy. |
| `-PSModuleInstall` | `String[]` | False | Specifies the value for PSModuleInstall. |
| `-Startnet` | `String` | False | Specifies the value for Startnet. |
| `-StartOSDCloud` | `String` | False | Specifies the value for StartOSDCloud. |
| `-StartOSDPad` | `String` | False | Specifies the value for StartOSDPad. |
| `-StartPSCommand` | `String` | False | Specifies the value for StartPSCommand. |
| `-StartURL` | `String` | False | Specifies the value for StartURL. |
| `-UpdateUSB` | `SwitchParameter` | False | Indicates whether to enable UpdateUSB. |
| `-Wallpaper` | `FileInfo` | False | Specifies the value for Wallpaper. |
| `-UseDefaultWallpaper` | `SwitchParameter` | False | Indicates whether to enable UseDefaultWallpaper. |
| `-Brand` | `String` | False | Specifies the value for Brand. |
| `-WorkspacePath` | `String` | False | Specifies the value for WorkspacePath. |
| `-WirelessConnect` | `SwitchParameter` | False | Indicates whether to enable WirelessConnect. |
| `-WifiProfile` | `FileInfo` | False | Specifies the value for WifiProfile. |
| `-Add7Zip` | `SwitchParameter` | False | Indicates whether to enable Add7Zip. |

## Examples

### Example
```powershell
-StartOSDCloudGUI
Runs Edit-OSDCloudWinPE with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Start-OSDCloudCLI

Starts the OSDCloud Windows 10 or 11 Build Process from the OSD Module or a GitHub Repository

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Starts the OSDCloud Windows 10 or 11 Build Process from the OSD Module or a GitHub Repository

## Syntax

### Default (Default)
```powershell
Start-OSDCloudCLI [-ComputerManufacturer <String>] [-ComputerProduct <String>] [-Firmware] [-Restart]
 [-Shutdown] [-Screenshot] [-SkipAutopilot] [-ZTI] [-OSName <String>] [-OSEdition <String>]
 [-OSLanguage <String>] [-OSActivation <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Legacy
```powershell
Start-OSDCloudCLI [-ComputerManufacturer <String>] [-ComputerProduct <String>] [-Firmware] [-Restart]
 [-Shutdown] [-Screenshot] [-SkipAutopilot] [-ZTI] [-OSVersion <String>] [-OSReleaseID <String>]
 [-OSEdition <String>] [-OSLanguage <String>] [-OSActivation <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### CustomImage
```powershell
Start-OSDCloudCLI [-ComputerManufacturer <String>] [-ComputerProduct <String>] [-Firmware] [-Restart]
 [-Shutdown] [-Screenshot] [-SkipAutopilot] [-ZTI] [-FindImageFile] [-ImageFileUrl <String>]
 [-OSImageIndex <Int32>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ComputerManufacturer` | `String` | False | Overrides the detected manufacturer used for driver pack matching. |
| `-ComputerProduct` | `String` | False | Overrides the detected product/system identifier used for driver pack matching. |
| `-Firmware` | `SwitchParameter` | False | Enables firmware catalog processing for the deployment workflow. |
| `-Restart` | `SwitchParameter` | False | Restarts the computer after deployment completes. |
| `-Shutdown` | `SwitchParameter` | False | Shuts down the computer after deployment completes. |
| `-Screenshot` | `SwitchParameter` | False | Captures screenshots during the workflow in WinPE. |
| `-SkipAutopilot` | `SwitchParameter` | False | Skips Autopilot tasks in the deployment process. |
| `-ZTI` | `SwitchParameter` | False | Enables zero-touch mode and suppresses disk wipe prompts. |
| `-OSName` | `String` | False | Default parameter set OS selection, for example 'Windows 11 25H2 x64'. |
| `-OSVersion` | `String` | False | Legacy parameter set operating system family. |
| `-OSReleaseID` | `String` | False | Legacy parameter set operating system release identifier. |
| `-OSEdition` | `String` | False | Target Windows edition. |
| `-OSLanguage` | `String` | False | Target Windows language/culture. |
| `-OSActivation` | `String` | False | Target activation channel: Retail or Volume. |
| `-FindImageFile` | `SwitchParameter` | False | CustomImage parameter set switch to locate a local WIM/ESD file. |
| `-ImageFileUrl` | `String` | False | CustomImage parameter set URL to download a WIM/ESD image. |
| `-OSImageIndex` | `Int32` | False | CustomImage parameter set image index. |

## Examples

### Example
```powershell
Start-OSDCloudCLI
Starts OSDCloud CLI interactively.
```

### Example
```powershell
Start-OSDCloudCLI -OSName 'Windows 11 25H2 x64' -OSEdition Enterprise -OSLanguage en-us
Starts OSDCloud CLI with explicit OS selections.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

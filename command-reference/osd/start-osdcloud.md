# Start-OSDCloud

Prepare and start an OSDCloud deployment session (selects image, language, edition and other options).

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Start-OSDCloud gathers system information, validates prerequisites (PowerShell version, network,
presence of required utilities), and prepares a global configuration used by the OSDCloud workflow.
It can select a Windows Feature Update image from local catalogs or an image URL, prompt the user
for OS version/build/edition/culture when needed, and then calls Invoke-OSDCloud to run the deployment.

The function supports three parameter sets:
- Default: Choose a Windows feature update by name (recommended for normal interactive use).
- Legacy: Older style parameters (OSVersion + OSBuild) for backward compatibility.
- CustomImage: Use a custom WIM/ESD image from disk or a provided URL.

## Syntax

### Default (Default)
```powershell
Start-OSDCloud [-Manufacturer <String>] [-Product <String>] [-Firmware] [-Restart] [-Shutdown] [-Screenshot]
 [-SkipAutopilot] [-SkipODT] [-ZTI] [-OSName <String>] [-OSEdition <String>] [-OSLanguage <String>]
 [-OSActivation <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Legacy
```powershell
Start-OSDCloud [-Manufacturer <String>] [-Product <String>] [-Firmware] [-Restart] [-Shutdown] [-Screenshot]
 [-SkipAutopilot] [-SkipODT] [-ZTI] [-OSVersion <String>] [-OSBuild <String>] [-OSEdition <String>]
 [-OSLanguage <String>] [-OSActivation <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### CustomImage
```powershell
Start-OSDCloud [-Manufacturer <String>] [-Product <String>] [-Firmware] [-Restart] [-Shutdown] [-Screenshot]
 [-SkipAutopilot] [-SkipODT] [-ZTI] [-FindImageFile] [-ImageFileUrl <String>] [-OSImageIndex <Int32>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Manufacturer` | `String` | False | (Optional) Computer manufacturer string. Automatically populated from Get-MyComputerManufacturer -Brief. |
| `-Product` | `String` | False | (Optional) Computer product string. Automatically populated from Get-MyComputerProduct. |
| `-Firmware` | `SwitchParameter` | False | Switch. When set, instructs the module to include firmware (MSC) catalog scanning. |
| `-Restart` | `SwitchParameter` | False | Switch. Restart the computer after the deployment finishes. |
| `-Shutdown` | `SwitchParameter` | False | Switch. Shutdown the computer after the deployment finishes. |
| `-Screenshot` | `SwitchParameter` | False | Switch. Capture screenshots during OSDCloud WinPE using Start-ScreenPNGProcess. Screenshots are saved to $env:TEMP\Screenshots by default. |
| `-SkipAutopilot` | `SwitchParameter` | False | Switch. Skip AutoPilot enrollment tasks during the workflow. |
| `-SkipODT` | `SwitchParameter` | False | Switch. Skip running the Office Deployment Tool (ODT) tasks. |
| `-ZTI` | `SwitchParameter` | False | Switch. Zero-touch install mode (ZTI). When set, disk wipes proceed automatically without prompting. |
| `-OSName` | `String` | False | (Default parameter set) A validated OS selection string such as 'Windows 11 25H2 x64'. If omitted the function prompts interactively (unless ZTI is used which selects sensible defaults). |
| `-OSVersion` | `String` | False | (Legacy parameter set) Operating system family, e.g. 'Windows 11' or 'Windows 10'. |
| `-OSBuild` | `String` | False | (Legacy parameter set) Operating system build (alias: Build) such as '25H2','24H2','23H2','22H2'. |
| `-OSEdition` | `String` | False | Edition of Windows to install (e.g. 'Enterprise', 'Pro', 'Home'). Affects edition mapping and activation type (Retail vs Volume). |
| `-OSLanguage` | `String` | False | Language/culture tag to install (for example 'en-us', 'fr-fr', 'zh-cn'). |
| `-OSActivation` | `String` | False | License type for the installation. Valid values are 'Retail' or 'Volume'. |
| `-FindImageFile` | `SwitchParameter` | False | (CustomImage parameter set) Switch to prompt for a WIM/ESD file on removable media. |
| `-ImageFileUrl` | `String` | False | (CustomImage parameter set) URL to download a custom image if not available locally. |
| `-OSImageIndex` | `Int32` | False | (CustomImage parameter set) Image index within a WIM/ESD. Default is 0. |

## Examples

### Example
```powershell
Start-OSDCloud
Interactive: choose image and options via menus.
```

### Example
```powershell
Start-OSDCloud -OSName 'Windows 11 25H2 x64' -OSEdition Enterprise -OSLanguage en-us -SkipAutopilot
Non-interactive: specify OS selection and suppress autopilot.
```

### Example
```powershell
Start-OSDCloud -FindImageFile -ImageFileUrl 'https://server.example.com/images/install.wim' -OSImageIndex 1
Use a custom image URL.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

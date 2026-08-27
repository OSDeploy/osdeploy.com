# Invoke-OSDCloudIPU

Starts an OSDCloud in-place upgrade workflow.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates elevation, inspects the current device and operating system, resolves the target feature update image, prepares any required driver pack content, and launches Windows Setup with the requested upgrade options.

## Syntax

```powershell
Invoke-OSDCloudIPU [-OSName <String>] [-Silent] [-SkipDriverPack] [-NoReboot] [-DownloadOnly]
 [-DiagnosticPrompt] [-SkipFinalize] [-Finalize] [-DynamicUpdate] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSName` | `String` | False | Specifies the target feature update image to download and install. |
| `-Silent` | `SwitchParameter` | False | Runs Windows Setup with the quiet UI mode. |
| `-SkipDriverPack` | `SwitchParameter` | False | Prevents driver pack download and integration even when a recommended driver pack is available. |
| `-NoReboot` | `SwitchParameter` | False | Prevents Windows Setup from rebooting after the down-level phase completes. |
| `-DownloadOnly` | `SwitchParameter` | False | Stops after downloading and preparing upgrade content without launching Setup. |
| `-DiagnosticPrompt` | `SwitchParameter` | False | Enables the Windows Setup diagnostic command prompt. |
| `-SkipFinalize` | `SwitchParameter` | False | Starts setup operations on the down-level OS without immediately initiating the offline phase. |
| `-Finalize` | `SwitchParameter` | False | Completes previously started setup operations and immediately reboots to start the offline phase. |
| `-DynamicUpdate` | `SwitchParameter` | False | Enables Windows Setup Dynamic Update so setup can search for and install updates during the upgrade. |

## Examples

### Example
```powershell
Invoke-OSDCloudIPU -OSName 'Windows 11 24H2 x64' -Silent -DynamicUpdate
Downloads the 24H2 x64 image and starts the upgrade with a quiet setup experience and Dynamic Update enabled.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://learn.microsoft.com/en-us/windows/deployment/upgrade/log-files](https://learn.microsoft.com/en-us/windows/deployment/upgrade/log-files)
* [https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options?view=windows-11](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options?view=windows-11)

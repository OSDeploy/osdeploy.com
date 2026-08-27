# Install-SystemFirmwareUpdate

Downloads and installs the system firmware update

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads the latest system firmware update from Microsoft Update Catalog and installs it on the running system.
Requires admin rights and PowerShell 5.1.

## Syntax

```powershell
Install-SystemFirmwareUpdate [[-DestinationDirectory] <String>] [-Force] [-Restart]
 [-ProgressAction <ActionPreference>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DestinationDirectory` | `String` | False | Directory where the firmware update will be downloaded. Default is C:\Drivers\SystemFirmwareUpdate |
| `-Force` | `SwitchParameter` | False | Required switch to perform the firmware update. Without this switch, the function only warns and exits. |
| `-Restart` | `SwitchParameter` | False | Restarts the computer automatically when a reboot is required after installation. |
| `-WhatIf` | `SwitchParameter` | False | Shows what would happen if the cmdlet runs. The cmdlet is not run. |
| `-Confirm` | `SwitchParameter` | False | Prompts you for confirmation before running the cmdlet. |

## Examples

### Example
```powershell
Install-SystemFirmwareUpdate
Downloads and installs the latest firmware update
```

### Example
```powershell
Install-SystemFirmwareUpdate -DestinationDirectory 'D:\Updates'
Downloads firmware update to D:\Updates and installs it
```

### Example
```powershell
Install-SystemFirmwareUpdate -Force -Restart
Downloads and installs the latest firmware update and restarts if required.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

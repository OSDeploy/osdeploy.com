# Initialize-OSDCloudStartnet

Initializes the OSDCloud startnet environment.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

This function initializes the OSDCloud startnet environment by performing the following tasks:
- Creates a log path if it does not already exist.
- Copies OSDCloud config startup scripts to the mounted WinPE.
- Initializes a splash screen if a SPLASH.JSON file is found in OSDCloud\Config.
- Initializes hardware devices.
- Initializes wireless network (optional).
- Initializes network connections.
- Updates PowerShell modules.

## Syntax

```powershell
Initialize-OSDCloudStartnet [-WirelessConnect] [-WifiProfile] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WirelessConnect` | `SwitchParameter` | False | Specifies whether to connect to a wireless network. If this switch is specified, the function will attempt to connect to a wireless network using the Start-WinREWiFi function. |
| `-WifiProfile` | `SwitchParameter` | False | No additional description provided. |

## Examples

### Example
```powershell
Initialize-OSDCloudStartnet -WirelessConnect
Initializes the OSDCloud startnet environment and attempts to connect to a wireless network.
```

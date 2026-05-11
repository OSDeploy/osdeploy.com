# Invoke-WinPEStartupManager

Invokes a WinPE startup utility action by Id.

| Property | Value                                       |
|----------|---------------------------------------------|
| Module   | OSDCloud                                    |
| Platform | WinPE only                                  |
| Requires | OSDCloud module, WinPE environment          |

{% hint style="info" %}
This function is called internally by `Invoke-WinPEStartup` to dispatch individual startup actions. It can also be called directly to run a specific startup action on demand.
{% endhint %}

## Description

Routes WinPE startup actions to the corresponding helper command by action Id. Supported actions include on-screen keyboard handling, PnP hardware and error display, network utilities, and module update operations.

## Syntax

```powershell
Invoke-WinPEStartupManager [-Id] <String> [[-Value] <String>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Id` | `String` | Yes | The startup action to invoke. Valid values: `OSK`, `DeviceErrors`, `DeviceHardware`, `Info`, `IPConfig`, `UpdateModule`, `WiFi`. |
| `-Value` | `String` | No | Optional value used by specific actions. For `UpdateModule`, set this to the module name to install or update. |

## Examples

```powershell
# Launch on-screen keyboard if no physical keyboard is detected
Invoke-WinPEStartupManager -Id OSK
```

```powershell
# Display non-OK PnP device status
Invoke-WinPEStartupManager -Id DeviceErrors
```

```powershell
# Display PnP device hardware details
Invoke-WinPEStartupManager -Id DeviceHardware
```

```powershell
# Show comprehensive device information
Invoke-WinPEStartupManager -Id Info
```

```powershell
# Launch IPConfig in a minimized window
Invoke-WinPEStartupManager -Id IPConfig
```

```powershell
# Start the Wi-Fi connection workflow
Invoke-WinPEStartupManager -Id WiFi
```

```powershell
# Update the OSDCloud module
Invoke-WinPEStartupManager -Id UpdateModule -Value OSDCloud
```

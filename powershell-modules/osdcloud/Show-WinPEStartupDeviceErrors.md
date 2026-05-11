# Show-WinPEStartupDeviceErrors

Displays WinPE Plug and Play devices with non-OK status.

| Property | Value                                       |
|----------|---------------------------------------------|
| Module   | OSDCloud                                    |
| Platform | WinPE only                                  |
| Requires | OSDCloud module, WinPE environment          |

{% hint style="warning" %}
This function exits the host process after displaying results. Do not call it in an interactive session where you need to continue running commands afterward.
{% endhint %}

## Description

Queries `Win32_PnPEntity` and displays devices where the status is not `OK`. Writes a warning that the window will close, displays results in table format, waits 5 seconds, then exits the host process.

## Syntax

```powershell
Show-WinPEStartupDeviceErrors
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Display detected PnP device errors during WinPE startup
Show-WinPEStartupDeviceErrors
```

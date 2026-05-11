# Show-WinPEStartupDevices

Displays the WinPE Plug and Play device inventory.

| Property | Value                                       |
|----------|---------------------------------------------|
| Module   | OSDCloud                                    |
| Platform | WinPE only                                  |
| Requires | OSDCloud module, WinPE environment          |

{% hint style="info" %}
This function also copies the source query command to the clipboard for reuse in the WinPE console.
{% endhint %}

## Description

Queries `Win32_PnPEntity` and displays device class, status, IDs, and manufacturer details in table format.

## Syntax

```powershell
Show-WinPEStartupDevices
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Display Plug and Play hardware information in WinPE
Show-WinPEStartupDevices
```

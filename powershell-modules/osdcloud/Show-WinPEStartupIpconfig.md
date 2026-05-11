# Show-WinPEStartupIpconfig

Displays IP configuration details in WinPE.

| Property | Value                                       |
|----------|---------------------------------------------|
| Module   | OSDCloud                                    |
| Platform | WinPE only                                  |
| Requires | OSDCloud module, WinPE environment          |

## Description

Runs `ipconfig /all` to display network adapter and IP addressing details. Typically used during WinPE startup troubleshooting to confirm network connectivity and address assignment.

## Syntax

```powershell
Show-WinPEStartupIpconfig
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Display full network adapter and IP configuration details in WinPE
Show-WinPEStartupIpconfig
```

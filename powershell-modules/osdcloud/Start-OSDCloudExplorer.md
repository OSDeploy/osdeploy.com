# Start-OSDCloudExplorer

Opens a graphical file browser for WinPE and WinRE environments.

| Property | Value                                               |
|----------|-----------------------------------------------------|
| Module   | OSDCloud                                            |
| Platform | WinPE / WinRE                                       |
| Requires | `WinPE-NetFX` and `WinPE-PowerShell` ADK packages  |

## Description

Provides a Windows Forms file browser with TreeView, ListView, Up navigation, and keyboard shortcuts. Designed for use in Windows PE and Windows RE where Windows Explorer is not available.

{% hint style="info" %}
The WinPE boot image must include the `WinPE-NetFX` and `WinPE-PowerShell` ADK optional components for this function to work.
{% endhint %}

## Syntax

```powershell
Start-OSDCloudExplorer
```

## Parameters

This function has no user-facing parameters.

{% hint style="info" %}
The internal `-DirectLaunch` parameter is used by the function to spawn a child process that runs the WinForms UI inline. It is hidden from tab-completion and not intended for direct use.
{% endhint %}

## Examples

```powershell
# Open the graphical file browser in WinPE or WinRE
Start-OSDCloudExplorer
```

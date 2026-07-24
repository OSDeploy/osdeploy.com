# Unblock-WindowsUpdate

Opens Windows Update and checks for WSUS configuration

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Opens Windows Update and checks for WSUS configuration

## Syntax

```powershell
Unblock-WindowsUpdate [-DisableWSUS] [-EnableDrivers] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DisableWSUS` | `SwitchParameter` | False | Sets the Group Policy 'Download repair content and optional features directly from Windows Update instead of Windows Server Update Services (WSUS)' Restarts the Windows Update Service This setting will be enabled after restart by Group Policy |
| `-EnableDrivers` | `SwitchParameter` | False | Allows Driver Updates in Windows Update |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

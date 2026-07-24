# Show-MsSettings

Opens the ms-setting: URI that is specified in the Setting parameter

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Opens the ms-setting: URI that is specified in the Setting parameter

## Syntax

```powershell
Show-MsSettings [[-Setting] <String>] [-DisableWSUS] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Setting` | `String` | False | No additional description provided. |
| `-DisableWSUS` | `SwitchParameter` | False | Sets the Group Policy 'Download repair content and optional features directly from Windows Update instead of Windows Server Update Services (WSUS)' Restarts the Windows Update Service This setting will be enabled after restart by Group Policy |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

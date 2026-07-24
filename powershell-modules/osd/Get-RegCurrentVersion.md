# Get-RegCurrentVersion

Returns the Registry Key values from HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns the Registry Key values from HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion for Online and Offline Windows Images

## Syntax

```powershell
Get-RegCurrentVersion [[-Path] <String>] [[-Property] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | False | Specifies the full path to the root directory of the offline Windows image that you will service. |
| `-Property` | `String` | False | No additional description provided. |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

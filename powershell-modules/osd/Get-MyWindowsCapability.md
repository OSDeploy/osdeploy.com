# Get-MyWindowsCapability

Gets MyWindowsCapability information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns MyWindowsCapability data for the current system or OSD session context.

## Syntax

### Online (Default)
```powershell
Get-MyWindowsCapability [-State <String>] [-Category <String>] [-Culture <String[]>] [-Like <String[]>]
 [-Match <String[]>] [-Detail] [-DisableWSUS] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Offline
```powershell
Get-MyWindowsCapability -Path <String> [-State <String>] [-Category <String>] [-Culture <String[]>]
 [-Like <String[]>] [-Match <String[]>] [-Detail] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | True | Specifies the Path to use when running Get-MyWindowsCapability. |
| `-State` | `String` | False | Specifies the State to use when running Get-MyWindowsCapability. |
| `-Category` | `String` | False | Specifies the Category to use when running Get-MyWindowsCapability. |
| `-Culture` | `String[]` | False | Specifies the Culture to use when running Get-MyWindowsCapability. |
| `-Like` | `String[]` | False | Specifies the Like to use when running Get-MyWindowsCapability. |
| `-Match` | `String[]` | False | Specifies the Match to use when running Get-MyWindowsCapability. |
| `-Detail` | `SwitchParameter` | False | Specifies the Detail to use when running Get-MyWindowsCapability. |
| `-DisableWSUS` | `SwitchParameter` | False | Specifies the DisableWSUS to use when running Get-MyWindowsCapability. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-MyWindowsCapability.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

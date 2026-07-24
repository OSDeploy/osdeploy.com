# Get-OSDWinEvent

Gets OSDWinEvent information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns OSDWinEvent data for the current system or OSD session context.

## Syntax

```powershell
Get-OSDWinEvent [[-Area] <String>] [[-DayCount] <Int32>] [[-LogName] <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Area` | `String` | False | Specifies the Area to use when running Get-OSDWinEvent. |
| `-DayCount` | `Int32` | False | Specifies the DayCount to use when running Get-OSDWinEvent. |
| `-LogName` | `String[]` | False | Specifies the LogName to use when running Get-OSDWinEvent. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-OSDWinEvent.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

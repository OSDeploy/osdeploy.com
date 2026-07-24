# Get-PowerSettingTurnMonitorOffAfter

Gets the active power plan monitor-off timeout in minutes.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns the "Turn off display after" timeout for the active power plan.
The function reads both AC (plugged in) and DC (battery) values from
power policy data in root\cimv2\power.

## Syntax

```powershell
Get-PowerSettingTurnMonitorOffAfter [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-PowerSettingTurnMonitorOffAfter
```

Returns a PSCustomObject with AC and DC monitor-off timeout values
in minutes.

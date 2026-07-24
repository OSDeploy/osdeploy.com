# Get-OSDMetrics

Retrieves metrics for the OSD PowerShell module and OSDCloud deployment methods.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

The Get-OSDMetrics script retrieves metrics for the OSD PowerShell module and OSDCloud deployment methods.
It displays the latest version of the OSD PowerShell module, the date it was published, and the number of times it has been installed or saved.
It also displays metrics for OSDCloud CLI, OSDCloud GUI, and OSDCloud Azure deployment methods, including the number of devices deployed, the current usage rate, and the number of devices deployed per day, week, month, and year.

## Syntax

```powershell
Get-OSDMetrics [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-OSDMetrics
This example retrieves metrics for the OSD PowerShell module and OSDCloud deployment methods.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-OSDPower

Displays Power Plan information using powercfg /LIST

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Displays Power Plan information using powercfg /LIST. 
Optionally Set an Active Power Plan

## Syntax

```powershell
Get-OSDPower [[-Property] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Property` | `String` | False | Powercfg option (Low, Balanced, High, LIST, QUERY) Default is LIST |

## Examples

### Example
```powershell
OSDPower
Returns Power Plan information using powercfg /LIST
Option 1: Get-OSDPower
Option 2: Get-OSDPower LIST
Option 3: Get-OSDPower -Property LIST
```

### Example
```powershell
OSDPower High
Sets the active Power Plan to High Performance
Option 1: Get-OSDPower High
Option 2: Get-OSDPower -Property High
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

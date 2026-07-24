# Get-OSDGather

Returns common OSD information as an ordered hash table

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns common OSD information as an ordered hash table

## Syntax

```powershell
Get-OSDGather [[-Property] <String>] [-Full] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Property` | `String` | False | Returns the Name Value |
| `-Full` | `SwitchParameter` | False | Returns additional CimInstance results |

## Examples

### Example
```powershell
OSDGather
Get-OSDGather
Returns the Gather Results
```

### Example
```powershell
$OSDGather = Get-OSDGather
$OSDGather.IsAdmin
$OSDGather.ComputerInfo
Returns the Gather Results saved in a Variable
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

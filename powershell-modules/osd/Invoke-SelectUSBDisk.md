# Invoke-SelectUSBDisk

Invokes SelectUSBDisk actions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Runs interactive or workflow-oriented SelectUSBDisk operations used by OSD tasks.

## Syntax

```powershell
Invoke-SelectUSBDisk [[-Input] <Object>] [[-MinimumSizeGB] <Int32>] [[-MaximumSizeGB] <Int32>] [-Skip]
 [-SelectOne] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Input` | `Object` | False | Specifies the Input to use when running Invoke-SelectUSBDisk. |
| `-MinimumSizeGB` | `Int32` | False | Specifies the MinimumSizeGB to use when running Invoke-SelectUSBDisk. |
| `-MaximumSizeGB` | `Int32` | False | Specifies the MaximumSizeGB to use when running Invoke-SelectUSBDisk. |
| `-Skip` | `SwitchParameter` | False | Specifies the Skip to use when running Invoke-SelectUSBDisk. |
| `-SelectOne` | `SwitchParameter` | False | Specifies the SelectOne to use when running Invoke-SelectUSBDisk. |

## Examples

### Example
```powershell
Demonstrates a common way to run Invoke-SelectUSBDisk.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

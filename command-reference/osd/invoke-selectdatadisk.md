# Invoke-SelectDataDisk

Invokes SelectDataDisk actions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Runs interactive or workflow-oriented SelectDataDisk operations used by OSD tasks.

## Syntax

```powershell
Invoke-SelectDataDisk [[-NotDiskNumber] <Int32>] [-Skip] [-SelectOne] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-NotDiskNumber` | `Int32` | False | Specifies the NotDiskNumber to use when running Invoke-SelectDataDisk. |
| `-Skip` | `SwitchParameter` | False | Specifies the Skip to use when running Invoke-SelectDataDisk. |
| `-SelectOne` | `SwitchParameter` | False | Specifies the SelectOne to use when running Invoke-SelectDataDisk. |

## Examples

### Example
```powershell
Demonstrates a common way to run Invoke-SelectDataDisk.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

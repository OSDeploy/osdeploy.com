# Invoke-SelectLocalDisk

Invokes SelectLocalDisk actions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Runs interactive or workflow-oriented SelectLocalDisk operations used by OSD tasks.

## Syntax

```powershell
Invoke-SelectLocalDisk [[-Input] <Object>] [-Skip] [-SelectOne] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Input` | `Object` | False | Specifies the Input to use when running Invoke-SelectLocalDisk. |
| `-Skip` | `SwitchParameter` | False | Specifies the Skip to use when running Invoke-SelectLocalDisk. |
| `-SelectOne` | `SwitchParameter` | False | Specifies the SelectOne to use when running Invoke-SelectLocalDisk. |

## Examples

### Example
```powershell
Demonstrates a common way to run Invoke-SelectLocalDisk.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

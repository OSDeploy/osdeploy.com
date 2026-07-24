# Invoke-SelectOSDVolume

Invokes SelectOSDVolume actions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Runs interactive or workflow-oriented SelectOSDVolume operations used by OSD tasks.

## Syntax

```powershell
Invoke-SelectOSDVolume [[-Input] <Object>] [[-MinimumSizeGB] <Int32>] [[-FileSystem] <String>] [-Skip]
 [-SelectOne] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Input` | `Object` | False | Specifies the Input to use when running Invoke-SelectOSDVolume. |
| `-MinimumSizeGB` | `Int32` | False | Specifies the MinimumSizeGB to use when running Invoke-SelectOSDVolume. |
| `-FileSystem` | `String` | False | Specifies the FileSystem to use when running Invoke-SelectOSDVolume. |
| `-Skip` | `SwitchParameter` | False | Specifies the Skip to use when running Invoke-SelectOSDVolume. |
| `-SelectOne` | `SwitchParameter` | False | Specifies the SelectOne to use when running Invoke-SelectOSDVolume. |

## Examples

### Example
```powershell
Demonstrates a common way to run Invoke-SelectOSDVolume.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

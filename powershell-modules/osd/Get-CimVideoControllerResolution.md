# Get-CimVideoControllerResolution

Returns CIM video controller resolution entries for the system display adapter.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Queries CIM_VideoControllerResolution, filters out low resolutions, and returns
either progressive or interlaced modes based on the selected switch.

## Syntax

```powershell
Get-CimVideoControllerResolution [-Interlaced] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Interlaced` | `SwitchParameter` | False | Returns interlaced resolutions when specified. By default, progressive resolutions are returned. |

## Examples

### Example
```powershell
Get-CimVideoControllerResolution
Returns progressive resolutions with a horizontal resolution of 800 or higher.
```

### Example
```powershell
Get-CimVideoControllerResolution -Interlaced
Returns interlaced resolutions with a horizontal resolution of 800 or higher.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Set-DisRes

Sets the primary display screen resolution.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Changes the primary display resolution to the specified width and height, or to a preset alias such as 720p, 1080p, 4k, or Restore.

## Syntax

```powershell
Set-DisRes [[-Width] <String>] [[-Height] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Width` | `String` | False | Target horizontal resolution, a preset alias, or Restore to return to the previous value captured in the current session. |
| `-Height` | `String` | False | Target vertical resolution. If omitted when Width is numeric or a preset alias, Height may be auto-selected from common aspect ratios. |

## Examples

### Example
```powershell
Set-DisRes -Width 1920 -Height 1080
Sets the primary display resolution to 1920x1080.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

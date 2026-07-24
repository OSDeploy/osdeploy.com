# Start-ScreenPNGProcess

Starts a background process to capture screenshots

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Launches a hidden PowerShell process that periodically captures screenshots and saves them to the specified directory.

## Syntax

```powershell
Start-ScreenPNGProcess [-Directory] <String> [[-Delay] <UInt32>] [[-Count] <UInt32>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Directory` | `String` | True | Directory where screenshots will be saved. This parameter is mandatory. |
| `-Delay` | `UInt32` | False | Delay in seconds between screenshots. Default is 2 seconds |
| `-Count` | `UInt32` | False | Total number of screenshots to capture. Default is 9999 |

## Examples

### Example
```powershell
Start-ScreenPNGProcess -Directory 'C:\Screenshots'
Starts capturing screenshots with default delay and count
```

### Example
```powershell
Start-ScreenPNGProcess -Directory 'C:\Screenshots' -Count 5 -Delay 3
Starts capturing 5 screenshots with 3-second intervals
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-ScreenPNG

Gets ScreenPNG information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns ScreenPNG data for the current system or OSD session context.

## Syntax

```powershell
Get-ScreenPNG [[-Directory] <String>] [[-Prefix] <String>] [[-Delay] <UInt32>] [[-Count] <UInt32>] [-Clipboard]
 [-Primary] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Directory` | `String` | False | Specifies the Directory to use when running Get-ScreenPNG. |
| `-Prefix` | `String` | False | Specifies the Prefix to use when running Get-ScreenPNG. |
| `-Delay` | `UInt32` | False | Specifies the Delay to use when running Get-ScreenPNG. |
| `-Count` | `UInt32` | False | Specifies the Count to use when running Get-ScreenPNG. |
| `-Clipboard` | `SwitchParameter` | False | Specifies the Clipboard to use when running Get-ScreenPNG. |
| `-Primary` | `SwitchParameter` | False | Specifies the Primary to use when running Get-ScreenPNG. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-ScreenPNG.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

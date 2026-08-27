# Test-WindowsImage

Tests WindowsImage conditions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Evaluates WindowsImage state and returns a validation result for scripting decisions.

## Syntax

```powershell
Test-WindowsImage [-ImagePath] <String> [-Index <UInt32>] [-Extension <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ImagePath` | `String` | True | Specifies the ImagePath to use when running Test-WindowsImage. |
| `-Index` | `UInt32` | False | Specifies the Index to use when running Test-WindowsImage. |
| `-Extension` | `String` | False | Specifies the Extension to use when running Test-WindowsImage. |

## Examples

### Example
```powershell
Demonstrates a common way to run Test-WindowsImage.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

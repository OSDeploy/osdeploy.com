# Test-WindowsImageMounted

Tests WindowsImageMounted conditions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Evaluates WindowsImageMounted state and returns a validation result for scripting decisions.

## Syntax

```powershell
Test-WindowsImageMounted [-ImagePath] <String> [-Index <UInt32>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ImagePath` | `String` | True | Specifies the ImagePath to use when running Test-WindowsImageMounted. |
| `-Index` | `UInt32` | False | Specifies the Index to use when running Test-WindowsImageMounted. |

## Examples

### Example
```powershell
Demonstrates a common way to run Test-WindowsImageMounted.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

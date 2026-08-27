# Test-WindowsPackageCAB

Tests WindowsPackageCAB conditions.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Evaluates WindowsPackageCAB state and returns a validation result for scripting decisions.

## Syntax

```powershell
Test-WindowsPackageCAB [-PackagePath] <String> [[-Path] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-PackagePath` | `String` | True | Specifies the PackagePath to use when running Test-WindowsPackageCAB. |
| `-Path` | `String` | False | Specifies the Path to use when running Test-WindowsPackageCAB. |

## Examples

### Example
```powershell
Demonstrates a common way to run Test-WindowsPackageCAB.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

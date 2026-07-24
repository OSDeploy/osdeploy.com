# Get-MyWindowsPackage

Gets MyWindowsPackage information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns MyWindowsPackage data for the current system or OSD session context.

## Syntax

### Online (Default)
```powershell
Get-MyWindowsPackage [-PackageState <String>] [-ReleaseType <String>] [-Category <String>]
 [-Culture <String[]>] [-Like <String[]>] [-Match <String[]>] [-Detail] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### Offline
```powershell
Get-MyWindowsPackage -Path <String> [-PackageState <String>] [-ReleaseType <String>] [-Category <String>]
 [-Culture <String[]>] [-Like <String[]>] [-Match <String[]>] [-Detail] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | True | Specifies the Path to use when running Get-MyWindowsPackage. |
| `-PackageState` | `String` | False | Specifies the PackageState to use when running Get-MyWindowsPackage. |
| `-ReleaseType` | `String` | False | Specifies the ReleaseType to use when running Get-MyWindowsPackage. |
| `-Category` | `String` | False | Specifies the Category to use when running Get-MyWindowsPackage. |
| `-Culture` | `String[]` | False | Specifies the Culture to use when running Get-MyWindowsPackage. |
| `-Like` | `String[]` | False | Specifies the Like to use when running Get-MyWindowsPackage. |
| `-Match` | `String[]` | False | Specifies the Match to use when running Get-MyWindowsPackage. |
| `-Detail` | `SwitchParameter` | False | Specifies the Detail to use when running Get-MyWindowsPackage. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-MyWindowsPackage.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

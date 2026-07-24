# Copy-PSModuleToFolder

Copies PowerShell modules to a destination module path.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Finds the latest installed version of each requested module and copies it to
the destination using the standard module\version folder layout.

## Syntax

```powershell
Copy-PSModuleToFolder [-Name] <String[]> [-Destination] <String> [-RemoveOldVersions]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Name` | `String[]` | True | One or more module names to copy. |
| `-Destination` | `String` | True | Destination root folder for copied modules. |
| `-RemoveOldVersions` | `SwitchParameter` | False | Removes existing module content from the destination before copying. |

## Examples

### Example
```powershell
Copy-PSModuleToFolder -Name OSD -Destination 'C:\Modules'
Copies the latest installed OSD module to C:\Modules\OSD\<version>.
```

### Example
```powershell
Copy-PSModuleToFolder -Name OSD,PackageManagement -Destination 'C:\Modules' -RemoveOldVersions
Removes existing destination module content and copies fresh module versions.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

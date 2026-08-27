# Get-OSDCloudModulePath

Returns the base directory path of the OSDCloud module.

| Property | Value           |
|----------|-----------------|
| Module   | OSDCloud        |
| Platform | Windows         |
| Output   | `System.String` |

## Description

Returns the file system path to the root folder where the OSDCloud module is installed. Useful for locating module-relative resources such as catalogs, templates, and support files.

## Syntax

```powershell
Get-OSDCloudModulePath
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Return the path to the OSDCloud module directory
Get-OSDCloudModulePath
```

Example output:

```
C:\Program Files\WindowsPowerShell\Modules\OSDCloud\1.0.0
```

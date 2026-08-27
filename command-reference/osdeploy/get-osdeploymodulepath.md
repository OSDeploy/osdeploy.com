# Get-OSDeployModulePath

Returns the file system path to the root directory of the OSDeploy module installation.

| Property | Value          |
|----------|----------------|
| Module   | OSDeploy       |
| Platform | Windows        |
| Output   | `System.String` |

## Description

Returns the absolute file system path to the root folder where the OSDeploy module is installed. This is useful for locating module-relative resources such as catalogs, templates, and support files.

## Syntax

```powershell
Get-OSDeployModulePath
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Return the path to the OSDeploy module directory
Get-OSDeployModulePath
```

Example output:

```
C:\Program Files\WindowsPowerShell\Modules\OSDeploy\26.5.1.1
```

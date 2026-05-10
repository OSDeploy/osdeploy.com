# Get-OSDeployModuleVersion

Returns the currently loaded version of the OSDeploy module.

| Property | Value            |
|----------|------------------|
| Module   | OSDeploy         |
| Platform | Windows          |
| Output   | `System.Version` |

## Description

Returns the version of the OSDeploy module currently loaded in the PowerShell session as a `System.Version` object. Useful for version checks, logging, and compatibility validation in scripts.

## Syntax

```powershell
Get-OSDeployModuleVersion
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Return the loaded OSDeploy module version
Get-OSDeployModuleVersion
```

Example output:

```
Major  Minor  Build  Revision
-----  -----  -----  --------
26     5      1      1
```

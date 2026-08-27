# Get-OSDCloudModuleVersion

Returns the version of the OSDCloud module.

| Property | Value            |
|----------|------------------|
| Module   | OSDCloud         |
| Platform | Windows          |
| Output   | `System.Version` |

## Description

Returns the currently loaded version of the OSDCloud module as a `System.Version` object. Useful for version checks, logging, and compatibility validation in scripts.

## Syntax

```powershell
Get-OSDCloudModuleVersion
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Return the loaded OSDCloud module version
Get-OSDCloudModuleVersion
```

Example output:

```
Major  Minor  Build  Revision
-----  -----  -----  --------
26     5      1      1
```

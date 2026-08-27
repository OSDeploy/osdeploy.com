# Get-OSDModuleVersion

Returns the version of the loaded OSD module.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses the current command invocation context to return the module version
object for the loaded OSD module.

## Syntax

```powershell
Get-OSDModuleVersion [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-OSDModuleVersion
```

Returns the currently loaded OSD module version.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

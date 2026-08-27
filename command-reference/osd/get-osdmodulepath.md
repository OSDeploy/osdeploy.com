# Get-OSDModulePath

Returns the base path of the loaded OSD module.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses the current command invocation context to return the module base path
where the OSD module is installed or loaded from.

## Syntax

```powershell
Get-OSDModulePath [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-OSDModulePath
```

Returns the OSD module installation path.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

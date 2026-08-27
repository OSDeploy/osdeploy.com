# Get-OSDModuleCache

Returns the OSD module cache directory path.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves the module base path from the current command context and appends
the cache child directory name.
This returns the expected cache folder path
for the installed OSD module.

## Syntax

```powershell
Get-OSDModuleCache [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-OSDModuleCache
```

Returns the full path to the OSD module cache directory.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

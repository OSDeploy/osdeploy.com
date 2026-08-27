# Get-OSDCoreOperatingSystems

Gets the core operating system catalog entries that OSD uses for offline media selection.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Imports the operating system catalog XML files stored under the module's core operating systems cache,
normalizes duplicate metadata, and returns a sorted list of operating system records with build,
architecture, language, activation, hash, and image metadata.

## Syntax

```powershell
Get-OSDCoreOperatingSystems [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-OSDCoreOperatingSystems
```

Returns all available core operating system records discovered in the module cache.

### Example
```powershell
Get-OSDCoreOperatingSystems | Where-Object Version -eq 'Windows 11'
```

Returns only Windows 11 operating system records.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

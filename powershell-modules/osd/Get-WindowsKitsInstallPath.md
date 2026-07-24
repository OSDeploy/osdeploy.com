# Get-WindowsKitsInstallPath

Retrieves the installation path of the Windows Kit directory.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the installation path of the Windows Kits (which includes ADK and other Windows development tools) from the registry.

## Syntax

```powershell
Get-WindowsKitsInstallPath [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-WindowsKitsInstallPath
Returns the Windows Kits installation directory path
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

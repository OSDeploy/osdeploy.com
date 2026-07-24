# Install-ModuleHPCMSL

Installs or updates the HP Client Management Script Library (HPCMSL) PowerShell module.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Ensures the HPCMSL module (version 1.8.5) is installed and up to date from the PowerShell Gallery.
If PowerShellGet 2.2.5 or later is not present, it is installed first.
Compares the installed HPCMSL version against the Gallery version and installs if missing or outdated.
Supports both WinPE and full Windows environments.
After installation, the module is imported into the global scope.

## Syntax

```powershell
Install-ModuleHPCMSL [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Install-ModuleHPCMSL
Installs or updates HPCMSL 1.8.5 for all users and imports it into the current session.
```

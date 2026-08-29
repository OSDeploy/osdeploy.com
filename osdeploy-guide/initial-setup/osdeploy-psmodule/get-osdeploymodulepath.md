---
description: Return and use the root path of the currently loaded OSDeploy module.
---

# Get-OSDeployModulePath

`Get-OSDeployModulePath` returns the absolute path to the root directory of the OSDeploy module loaded in the current PowerShell session. Use the returned path to locate module-relative catalogs, configuration files, templates, and other resources without assuming where PowerShell installed the module.

## Requirements

Install OSDeploy and run the command from PowerShell 7.6 or later. See [Module Setup](./) for installation and import instructions.

Import OSDeploy before using the function in a script that must control which installed module version is loaded:

```powershell
Import-Module -Name OSDeploy -Force
```

{% hint style="warning" %}
The function returns the path of the loaded OSDeploy module, which is not necessarily the newest version installed on the computer. Import the required version explicitly when a script depends on resources from a specific release.
{% endhint %}

## Parameters

This function has no function-specific parameters. It supports standard PowerShell common parameters through `CmdletBinding`, but they do not change path selection.

## Examples

### Return the loaded module path

Return the root directory of the OSDeploy module loaded in the current session:

```powershell
Get-OSDeployModulePath
```

### Build a path to a module resource

Use `Join-Path` to create a platform-safe path to `core\module.json`:

```powershell
$ModulePath = Get-OSDeployModulePath
$ModuleJsonPath = Join-Path -Path $ModulePath -ChildPath 'core\module.json'

$ModuleJsonPath
```

### Read module configuration

Locate and parse the module configuration as structured JSON:

```powershell
$ModuleJsonPath = Join-Path `
	-Path (Get-OSDeployModulePath) `
	-ChildPath 'core\module.json'

$ModuleConfiguration = Get-Content -Path $ModuleJsonPath -Raw |
	ConvertFrom-Json

$ModuleConfiguration
```

### Verify the loaded installation location

Compare the function result with the module metadata reported by `Get-Module`:

```powershell
$ModulePath = Get-OSDeployModulePath
$LoadedModule = Get-Module -Name OSDeploy

[PSCustomObject]@{
	FunctionPath = $ModulePath
	ModuleBase   = $LoadedModule.ModuleBase
	Matches      = $ModulePath -eq $LoadedModule.ModuleBase
}
```

### Select and import a specific installed version

List installed versions, import the required release, and then return its root path:

```powershell
Get-Module -Name OSDeploy -ListAvailable |
	Sort-Object -Property Version -Descending |
	Select-Object -Property Name, Version, ModuleBase

Import-Module -Name OSDeploy -RequiredVersion '26.8.0' -Force
Get-OSDeployModulePath
```

Replace `26.8.0` with a version installed on the computer.

## Path Resolution

The function reads the `ModuleBase` property from the module associated with the running command. It does not search `PSModulePath`, inspect other installed releases, or choose the newest available version.

The returned path therefore follows the module import performed by PowerShell. When multiple OSDeploy versions are installed side by side, use `Import-Module -RequiredVersion` before calling the function to select a specific release.

Use `Join-Path` for child resources instead of concatenating strings. The returned value identifies the module root and does not include a trailing resource path.

## Session Behavior

The function calls the shared OSDeploy banner helper before returning the path. On its first use in a module session, the helper records that the banner has been displayed. The current helper does not write banner text and does not add output to the success pipeline.

The function does not modify files, install modules, or change the current location. It does not support `-WhatIf` or `-Confirm` because it performs no resource mutation.

## Output

The function returns one `System.String` containing the absolute `ModuleBase` path of the loaded OSDeploy module.

Assign the result directly when another command requires a path:

```powershell
$OSDeployModulePath = Get-OSDeployModulePath
```

See [Module Setup](./) for the installation workflow or the [Get-OSDeployModulePath command reference](../../../command-reference/osdeploy/get-osdeploymodulepath.md) for compact syntax and output information.

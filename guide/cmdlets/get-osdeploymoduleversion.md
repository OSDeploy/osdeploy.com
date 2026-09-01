---
description: >-
  Return, compare, and format the version of the currently loaded OSDeploy
  module.
---

# Get-OSDeployModuleVersion

`Get-OSDeployModuleVersion` returns the version of the OSDeploy module loaded in the current PowerShell session. The result is a `System.Version` object, so scripts can compare releases by version components instead of comparing version text.

## Requirements

Install OSDeploy and run the command from PowerShell 7.6 or later. See [Module Setup](../requirements/powershell-modules.md) for installation and import instructions.

Import OSDeploy before using the function in a script that must control which installed module version is loaded:

```powershell
Import-Module -Name OSDeploy -Force
```

{% hint style="warning" %}
The function returns the loaded module version, which is not necessarily the newest version installed on the computer. Import the required version explicitly before performing compatibility checks.
{% endhint %}

## Parameters

`Get-OSDeployModuleVersion` has no function-specific parameters and does not accept pipeline input. It supports PowerShell common parameters through `CmdletBinding`, but they do not change version resolution.

## Examples

### Return the loaded version

Return the version of the OSDeploy module loaded in the current session:

```powershell
Get-OSDeployModuleVersion
```

PowerShell applies its default formatting to the returned `System.Version` object.

### Test a minimum required version

Stop a script when the loaded module is older than the required release:

```powershell
$RequiredVersion = [System.Version]'26.8.0'
$LoadedVersion = Get-OSDeployModuleVersion

if ($LoadedVersion -lt $RequiredVersion) {
	throw "OSDeploy $RequiredVersion or later is required. Loaded version: $LoadedVersion"
}
```

### Test a supported version range

Run a workflow only when the loaded version is within a validated range:

```powershell
$MinimumVersion = [System.Version]'26.8.0'
$MaximumVersion = [System.Version]'27.0.0'
$LoadedVersion = Get-OSDeployModuleVersion

if ($LoadedVersion -lt $MinimumVersion -or $LoadedVersion -ge $MaximumVersion) {
	throw "Supported OSDeploy versions are $MinimumVersion through versions earlier than $MaximumVersion."
}
```

### Format the version for logging

Convert the version object to its standard string representation when writing text logs or environment values:

```powershell
$VersionText = (Get-OSDeployModuleVersion).ToString()

"Loaded OSDeploy version: $VersionText"
```

### Inspect individual version components

Capture the result and inspect its stable `System.Version` properties:

```powershell
$Version = Get-OSDeployModuleVersion

$Version | Select-Object -Property Major, Minor, Build, Revision
```

### Compare loaded and installed versions

Show the loaded version alongside every OSDeploy version available through `PSModulePath`:

```powershell
$LoadedVersion = Get-OSDeployModuleVersion
$InstalledVersions = Get-Module -Name OSDeploy -ListAvailable |
	Sort-Object -Property Version -Descending

[PSCustomObject]@{
	LoadedVersion    = $LoadedVersion
	NewestInstalled = $InstalledVersions[0].Version
	UpdateAvailable = $InstalledVersions[0].Version -gt $LoadedVersion
}
```

## Version Resolution

The function reads the `Version` property from the module associated with the running command. It does not query PowerShell Gallery, search for other installed releases, or select the newest version available on the computer.

When multiple OSDeploy versions are installed side by side, use `Import-Module -RequiredVersion` to load the release that the current workflow requires:

```powershell
Import-Module -Name OSDeploy -RequiredVersion '26.8.0' -Force
Get-OSDeployModuleVersion
```

## Version Comparisons

Compare the returned object with another `System.Version` value. This preserves numeric component ordering; for example, version `26.10.0` correctly evaluates as newer than `26.9.0`.

Avoid converting versions to strings before comparing them. String comparisons use text ordering and can produce incorrect results for numeric version components.

## Session Behavior

The function does not search installed releases, query PowerShell Gallery, install, update, or reload OSDeploy. It does not support `-WhatIf` or `-Confirm` because it does not declare `SupportsShouldProcess`.

## Output

The function returns one `System.Version` object for the loaded OSDeploy module.

| Property        | Description                                               |
| --------------- | --------------------------------------------------------- |
| `Major`         | Major version component.                                  |
| `Minor`         | Minor version component.                                  |
| `Build`         | Build version component, or `-1` when it is undefined.    |
| `Revision`      | Revision version component, or `-1` when it is undefined. |
| `MajorRevision` | High 16 bits of the revision number.                      |
| `MinorRevision` | Low 16 bits of the revision number.                       |

See [Module Setup](../requirements/powershell-modules.md) for the installation workflow or the [Get-OSDeployModuleVersion command reference](../../command-reference/osdeploy/get-osdeploymoduleversion.md) for compact syntax and output information.

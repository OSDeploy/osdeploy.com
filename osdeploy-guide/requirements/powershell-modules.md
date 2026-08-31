---
description: >-
  Install and verify the PowerShell modules required for current OSDeploy and
  OSDCloud workflows.
---

# PowerShell Modules

Install `OSDeploy` and `OSDCloud` from PowerShell Gallery on the OSDeploy PC, then verify that PowerShell can discover both modules and load the current OSDeploy commands.

{% hint style="info" %}
Install `OSD` only when a workflow requires legacy OSD or OSDCloud v1 commands. Use the `OSDCloud` module for current OSDCloud deployments.
{% endhint %}

## Install the Modules

{% stepper %}
{% step %}
### Confirm the Requirements

Run the commands on a Windows 11 OSDeploy PC from an elevated PowerShell 7.6 or later session. Confirm that the current session uses PowerShell Core:

```powershell
$PSVersionTable | Select-Object PSEdition, PSVersion
```

Confirm that `PSEdition` is `Core` and `PSVersion` is `7.6` or later before continuing.
{% endstep %}

{% step %}
### Install the Required Modules

Install the latest `OSDeploy` and `OSDCloud` releases from PowerShell Gallery:

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

`OSDeploy` creates and maintains OSDeploy Core and builds OSDCloud boot images on the OSDeploy PC. `OSDCloud` provides the current deployment commands used in Windows PE. The commands also update an existing installation when PowerShell Gallery contains a newer release.
{% endstep %}

{% step %}
### Install the Optional Legacy Module

Install `OSD` only when a deployment workflow requires commands that are not included in the current `OSDCloud` module:

```powershell
Install-Module -Name OSD -Force -SkipPublisherCheck
```
{% endstep %}

{% step %}
### Verify the Installation

Confirm that PowerShell can discover the required modules and display the newest installed version of each module:

```powershell
$RequiredModules = 'OSDeploy', 'OSDCloud'

foreach ($ModuleName in $RequiredModules) {
	$InstalledModule = Get-Module -Name $ModuleName -ListAvailable |
		Sort-Object -Property Version -Descending |
		Select-Object -First 1

	if (-not $InstalledModule) {
		throw "The required $ModuleName module is not installed."
	}

	$InstalledModule | Select-Object -Property Name, Version, Path
}
```

Import the newest installed OSDeploy module and return its loaded version:

```powershell
Import-Module -Name OSDeploy -Force
Get-OSDeployModuleVersion
```

`Get-OSDeployModuleVersion` returns the version loaded in the current session. It does not search for a newer installed or Gallery version.
{% endstep %}
{% endstepper %}

See the detailed guides for [Get-OSDeployModulePath](../advanced/get-osdeploymodulepath.md) and [Get-OSDeployModuleVersion](../advanced/get-osdeploymoduleversion.md), or use their compact [path](../../command-reference/osdeploy/get-osdeploymodulepath.md) and [version](../../command-reference/osdeploy/get-osdeploymoduleversion.md) command references. Continue to [Community Registration](../registration/).

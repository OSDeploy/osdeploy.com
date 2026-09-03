---
description: >-
  Install the PowerShell modules required for current OSDeploy and OSDCloud
  workflows.
---

# PowerShell Modules

Install `OSDeploy` and `OSDCloud` from PowerShell Gallery on the OSDeploy PC.

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
{% endstepper %}

See the detailed guides for [Get-OSDeployModulePath](../cmdlets/get-osdeploymodulepath.md) and [Get-OSDeployModuleVersion](../cmdlets/get-osdeploymoduleversion.md), or use their compact [path](../../command-reference/osdeploy/get-osdeploymodulepath.md) and [version](../../command-reference/osdeploy/get-osdeploymoduleversion.md) command references. A valid Recast Software Community License is required while the OSDeploy module is in preview. Complete [Community Registration](../registration.md) before running OSDeploy commands.

# Install PowerShell Modules

Install the Recast OSDeploy and Recast OSDCloud modules on the OSDeploy PC. Install the OSD module only when a workflow requires its legacy commands.

| Module          | Purpose                                                                                | Requirement |
| --------------- | -------------------------------------------------------------------------------------- | ----------- |
| Recast OSDeploy | Creates and maintains OSDeploy Core and builds OSDCloud boot images on the OSDeploy PC | Required    |
| Recast OSDCloud | Provides the current OSDCloud deployment commands used in Windows PE                   | Required    |
| OSD             | Provides legacy OSD and OSDCloud v1 commands                                           | Optional    |

{% hint style="info" %}
Run all commands on this page from an elevated PowerShell 7 session. Confirm that `$PSVersionTable.PSEdition` returns `Core` before continuing.
{% endhint %}

## Install the Required Modules

Install the latest Recast OSDeploy and Recast OSDCloud releases from PowerShell Gallery:

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

The commands also update an existing installation when a newer release is available.

## Install the Optional OSD Module

Install OSD only when a deployment workflow requires commands that are not included in the current OSDCloud module:

```powershell
Install-Module -Name OSD -Force -SkipPublisherCheck
```

For current OSDCloud deployments, use the OSDCloud module instead of the legacy OSDCloud v1 implementation in OSD.

## Verify the Modules

Confirm that the required modules are available and display the newest installed version of each module:

```powershell
$RequiredModules = 'OSDeploy', 'OSDCloud'

foreach ($ModuleName in $RequiredModules) {
	$InstalledModule = Get-Module -Name $ModuleName -ListAvailable |
		Sort-Object Version -Descending |
		Select-Object -First 1

	if (-not $InstalledModule) {
		throw "The required $ModuleName module is not installed."
	}

	$InstalledModule | Select-Object Name, Version, Path
}
```

Import OSDeploy and confirm that its version command is available:

```powershell
Import-Module -Name OSDeploy -Force
Get-OSDeployModuleVersion
```

Continue to [Registration](community-registration.md).

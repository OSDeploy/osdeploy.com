---
description: David Segura and Mike Marable
---

# Autopilot App Registration



This process will create an Azure Active Directory App Registration which you can then use with Get-WindowsAutopilotInfo to Autopilot register a device

{% embed url="https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app" %}

I originally discovered this solution from [MSP Automator](https://twitter.com/ceejplaysgames)

{% embed url="https://mspautomator.com/2021/10/03/automated-autopilot-enrollment-using-powershell-and-ninjarmm" %}

## Get-WindowsAutoPilotInfo Snippet

Use the following snippets as an example of how to PowerShell register a device using Get-WindowsAutoPilotInfo and an App Registration

```powershell
$TenantId = ''
$AppId = ''
$AppSecret = ''
$GroupTag = ''
Get-WindowsAutoPilotInfo -Online -TenantId $TenantId -AppId $AppId -AppSecret $AppSecret -GroupTag $GroupTag
```

### Splatting

```powershell
$AutopilotParams = @{
    Online = $true
    TenantId = ''
    AppId = ''
    AppSecret = ''
    GroupTag = 'YourGroupTag'
}
Get-WindowsAutoPilotInfo @AutopilotParams
```

## Create an App Registration

Start by creating an App Registration in Azure Active Directory for Single Tenant.  The name really doesn't matter, but be descriptive

![](<../.gitbook/assets/image (703).png>)

## API Permissions

The following API permission need to be set to allow Autopilot Registration with Get-WindowsAutopilotInfo.  You will need to **Grant admin consent** for your App Registration

![](<../.gitbook/assets/image (699).png>)

### Manifest

It's much easier to edit the **requiredResourceAccess** configuration in the App Registration Manifest by copying what I have here

```
"requiredResourceAccess": [
	{
		"resourceAppId": "00000003-0000-0000-c000-000000000000",
		"resourceAccess": [
			{
				"id": "e1fe6dd8-ba31-4d61-89e7-88639da4683d",
				"type": "Scope"
			},
			{
				"id": "243333ab-4d21-40cb-a475-36241daa0842",
				"type": "Role"
			},
			{
				"id": "5ac13192-7ace-4fcf-b828-1a26f28068ee",
				"type": "Role"
			}
		]
	}
],
```

## Certificates & secrets

Create a new Client secret and copy the Value

![](<../.gitbook/assets/image (704).png>)

## PowerShell Script

Gather your Application ID, and Tenant ID.  Those will be used as values to pass to Get-WindowsAutopilotInfo

![](<../.gitbook/assets/image (698).png>)

## Get-WindowsAutoPilotInfo

With all the proper values in place, you can compose a PowerShell script to register an Autopilot Device.  This example has a GroupTag of Enterprise

```powershell
$AutopilotParams = @{
    Online = $true
    TenantId = 'xxxxxxxx-f4bd-4048-b6cd-42db00a0bf3a'
    AppId = 'xxxxxxxx-fb7f-4470-a55e-ef1e7a0fa7ea'
    AppSecret = 'xxxxx~JQRdzKEM3_KP.ooFnk5pkeBcLDj2m..'
    GroupTag = 'Enterprise'
    Assign = $true
}
Get-WindowsAutoPilotInfo @AutopilotParams
```

## Key Vault

You can convert the PowerShell script to an Azure Key Vault Secret by copying the script to the Clipboard (yes, I know the screenshot needs to be updated), and yes you will have to create the KeyVault separately.

```
Set-CloudSecret -VaultName mmsmoa -Name AutopilotJoinApp -Clipboard
```

![](<../.gitbook/assets/image (701).png>)

## OOBE

You can now register a device in Autopilot with the following command if you have a KeyVault set

```powershell
#OSD Module using Device Code Flow
Invoke-CloudSecret mmsmoa AutopilotJoinApp

#No OSD Module
Install-Module Az.KeyVault -Force
Connect-AzAccount
Invoke-Expression (Get-AzKeyVaultSecret -VaultName mmsmoa -Name AutopilotJoinApp -AsPlainText)
```

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

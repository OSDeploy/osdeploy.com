# Invoke-AzOSDAzureConfig

Deploy OSDCloud Azure infrastructure with Bicep or Terraform.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Prepares the local OSDCloud workspace, installs the required IaC tools, authenticates to Azure
or the Azure CLI based on the selected parameter set, and deploys either the Bicep template or
the Terraform configuration in C:\OSDCloud.

## Syntax

### Bicep
```powershell
Invoke-AzOSDAzureConfig [-Location <Object>] [-ResourceGroupName <String>] [-AzOSDUserNameStart <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Terraform
```powershell
Invoke-AzOSDAzureConfig [-UseTerraform <Boolean>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Location` | `Object` | False | Azure region used by the Bicep deployment path. |
| `-ResourceGroupName` | `String` | False | Name of the resource group created and deployed by the Bicep path. |
| `-AzOSDUserNameStart` | `String` | False | Optional prefix passed through the Bicep parameter set for related OSDCloud Azure workflows. |
| `-UseTerraform` | `Boolean` | False | Select the Terraform deployment path. |

## Examples

### Example
```powershell
Invoke-AzOSDAzureConfig -Location eastus -ResourceGroupName rg-osdcloud
Runs the Bicep deployment path for the selected Azure region and resource group.
```

### Example
```powershell
Invoke-AzOSDAzureConfig -UseTerraform $true
Runs the Terraform deployment path from C:\OSDCloud.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Invoke-AzOSDAzureConfig.md](https://github.com/OSDeploy/OSD/blob/master/docs/Invoke-AzOSDAzureConfig.md)

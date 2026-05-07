---
description: 22.1.17
---

# Azure Key Vault Secret

{% embed url="https://azure.microsoft.com/en-us/services/key-vault" %}

{% embed url="https://docs.microsoft.com/en-us/azure/key-vault/secrets/about-secrets" %}

{% embed url="https://docs.microsoft.com/en-us/azure/key-vault/secrets/multiline-secrets" %}

Azure Key Vault makes it possible to securely store a String (or PowerShell Script) behind Azure Active Directory (Azure AD) authentication.

## Azure Key Vault Secret RBAC Roles

There are two RBAC roles that are needed, depending if you need Read or Read/Write access to Azure Key Vault

```
Key Vault Secrets Reader
Read secret contents.
Only works for key vaults that use the 'Azure role-based access control' permission model.

Key Vault Secrets Officer
Perform any action on the secrets of a key vault, except manage permissions.
Only works for key vaults that use the 'Azure role-based access control' permission model.
```

## PowerShell Modules

The following PowerShell Modules will be needed for Creating and Reading an Azure Key Vault Secret

* Az.Accounts
* Az.KeyVault

## Set-AzKeyVaultSecret

{% embed url="https://docs.microsoft.com/en-us/powershell/module/az.keyvault/set-azkeyvaultsecret?view=azps-7.1.0" %}

For this task, I decided to use `Invoke-RestMethod` to read a GitHub Gist as my string

```
$VaultName = 'PSCloudScript'
$Name = 'KeyVaultSecretTest'
$Uri = 'https://gist.githubusercontent.com/OSDeploy/5754963498d77bc254fbe1436af3cb7d/raw/Test-PSCloudScriptAzKeyVaultSecret.ps1'
$RawString = Invoke-RestMethod -Uri $Uri
$SecretValue = ConvertTo-SecureString -String $RawString -AsPlainText -Force
Set-AzKeyVaultSecret -VaultName $VaultName -Name $Name -SecretValue $SecretValue
```

![](<../../.gitbook/assets/image (222).png>)

Once this was complete, I verified in Azure Portal that my Key Vault Secret was created.  I also pressed the 'Show Secret Value' button and verified that my full script was saved as a Secret

![](<../../.gitbook/assets/image (266).png>)

## Get-AzKeyVaultSecret

{% embed url="https://docs.microsoft.com/en-us/powershell/module/az.keyvault/get-azkeyvaultsecret?view=azps-7.1.0" %}

I tested reading the Key Vault Secret with my Tech account and using `Get-AzKeyVaultSecret` returned the Key Vault Secret Object

![](<../../.gitbook/assets/image (330).png>)

To view the Secret, I added the `-AddPlainText` parameter which returned the PowerShell script.  Finally I tested passing this to `Invoke-Expression` to get the PowerShell script I saved executed

![](<../../.gitbook/assets/image (269).png>)

## Summary

This method adds the security of Azure with easy to remember words to execute a PowerShell Script in the Cloud

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}


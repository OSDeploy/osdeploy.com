---
description: 22.1.17
---

# PSCloudScript

{% hint style="info" %}
**I'm still adding content to this guide, so please be patient**
{% endhint %}

I've been spending a fair amount lately playing around with running PowerShell scripts that are saved on the internet and I thought it would be helpful to share what I have leared on this ... so this guide will be focused on executing PowerShell scripts that are saved on the Internet, what I call a **PSCloudScript**

## Method

The method for running a PowerShell script on the internet is to get the raw content of a URL using `Invoke-RestMethod`, and then execute the result using `Invoke-Expression`

```powershell
Invoke-RestMethod -Uri $Uri | Invoke-Expression
```

The following command lines are identical to the method above, just formatted differently.  My preference is **`iex(irm $Uri)`** as it is short and works for both PowerShell and CMD with minimal effort

{% code title="PowerShell" %}
```powershell
#full command
Invoke-Expression -Command (Invoke-RestMethod -Uri $Uri)

#using alias
iex(irm $Uri)

#pipeline
Invoke-RestMethod -Uri $Uri | Invoke-Expression

#pipeline using alias
irm $Uri | iex
```
{% endcode %}

{% code title="Command Prompt" %}
```batch
rem full command
powershell Invoke-Expression -Command (Invoke-RestMethod -Uri $Uri)

rem using alias
powershell iex(irm $Uri)
```
{% endcode %}

## KeyVault Secret Method

I'm jumping ahead here, but here is the command line for executing a PowerShell script saved as an Azure Key Vault Secret

{% code title="PowerShell" %}
```powershell
iex(AzKeyVaultSecret $VaultName $SecretName -As)
```
{% endcode %}

## Content

{% content-ref url="ps-cmdlets.md" %}
[ps-cmdlets.md](ps-cmdlets.md)
{% endcontent-ref %}

{% content-ref url="github-gist.md" %}
[github-gist.md](github-gist.md)
{% endcontent-ref %}

{% content-ref url="github-git-repo.md" %}
[github-git-repo.md](github-git-repo.md)
{% endcontent-ref %}

{% content-ref url="content-type-or-azure-static-web-app.md" %}
[content-type-or-azure-static-web-app.md](content-type-or-azure-static-web-app.md)
{% endcontent-ref %}

{% content-ref url="command-shortening.md" %}
[command-shortening.md](command-shortening.md)
{% endcontent-ref %}

{% content-ref url="azure-key-vault-secret.md" %}
[azure-key-vault-secret.md](azure-key-vault-secret.md)
{% endcontent-ref %}

{% content-ref url="osd-powershell-module.md" %}
[osd-powershell-module.md](osd-powershell-module.md)
{% endcontent-ref %}

{% content-ref url="pscloudscript-examples/" %}
[pscloudscript-examples](pscloudscript-examples/)
{% endcontent-ref %}

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}


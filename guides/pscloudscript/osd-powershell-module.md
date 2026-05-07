# OSD PowerShell Module

The OSD PowerShell Module contains some functions that can be helpful in working with a PSCloudScript

```powershell
Install-Module OSD -Force
```

## Get-GithubRawUrl

This function makes it easy to get the proper GitHub Raw URL for using with `Invoke-RestMethod` which can then be passed to `Invoke-Expression`

![](<../../.gitbook/assets/image (178).png>)

## Get-GithubRawContent

This function will resolve a GitHub URL to the GitHub Raw Url if necessary and return the content which can easily be passed to `Invoke-Expression`

![](<../../.gitbook/assets/image (181).png>)

## ConvertTo-PSKeyVaultSecret

{% hint style="info" %}
**Requires PowerShell Module OSD 22.1.18+**
{% endhint %}

This function should make it easier to create an Azure Key Vault Secret.  Make sure your PowerShell session is authenticated to Azure AD using an account with proper Azure Key Vault RBAC access

### FromUriContent (ParameterSet)

In this example a provided URL content is saved as a Key Vault Secret. If the content is in a GutHub Repo or GitHub Gist, the Raw URL will be resolve automatically

```powershell
ConvertTo-PSKeyVaultSecret -VaultName $VaultName -Name $SecretName -Uri $GitHubUri
```

![](<../../.gitbook/assets/image (204).png>)

One thing to mention is that a GitHub Gist with multiple files will be combined automatically into a single Azure Key Vault Secret

![](<../../.gitbook/assets/image (265).png>)

![](<../../.gitbook/assets/image (231).png>)

### FromClipboard (ParameterSet)

You can simply Copy some text with `Ctrl+C` (which sets this in your Clipboard to Paste) and use the `-Clipboard` switch to convert the last item in your Clipboard as an Azure Key Vault Secret

```powershell
ConvertTo-PSKeyVaultSecret -VaultName $VaultName -Name $SecretName -Clipboard
```

![](<../../.gitbook/assets/image (271).png>)

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}


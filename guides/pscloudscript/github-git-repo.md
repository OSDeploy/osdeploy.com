# GitHub Git Repo

Another way to save a PowerShell script online is to save it in a GitHub Git Repo.  In this example, I have created a GitHub Repo at this link

{% embed url="https://github.com/OSDeploy/PSCloudScript" %}

In this Repo, I have a script called Test-PSCloudScriptGit.ps1.  The link below is not the Raw link, so I'll need to get that

{% embed url="https://github.com/OSDeploy/PSCloudScript/blob/main/Test-PSCloudScriptGit.ps1" %}

## Get the Raw URL

For a file in a GitHub Repo, pressing the Raw button will open a new webpage that contains the Raw URL that you can link to

![](<../../.gitbook/assets/image (169).png>)

![](<../../.gitbook/assets/image (315).png>)

If you have the OSD PowerShell Module 22.1.15+ then you can use the `Get-GithubRawUrl` function to resolve the proper Raw URL for you

```powershell
PS C:\> Get-GithubRawUrl https://github.com/OSDeploy/PSCloudScript/blob/main/Test-PSCloudScriptGit.ps1
https://raw.githubusercontent.com/OSDeploy/PSCloudScript/main/Test-PSCloudScriptGit.ps1
```

## Execution

Execution is simple.  Perform an Invoke-RestMethod on the Raw URL and then perform an Invoke-Expression on the Raw URL content.  The example below shows a few different ways you can do this and tests the function that was contained in the Script

![](<../../.gitbook/assets/image (214).png>)

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

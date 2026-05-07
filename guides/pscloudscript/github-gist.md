---
description: 22.1.16
---

# GitHub Gist

An easy way to get a PowerShell script online is by saving it in a GitHub Gist

{% embed url="https://gist.github.com" %}

In this example, I'll create and save a Public Gist.  You can save it as a Private Gist, all that matters is that anyone with the URL has access, so don't consider it an option for saving Secure content like passwords

![](<../../.gitbook/assets/image (371).png>)

Once it is saved you should have the Git Editor URL

{% embed url="https://gist.github.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb" %}

## Gist Editing in Visual Studio Code

There are several Visual Studio Code extensions that can help you manage your GitHub gists, but one that really stands out is GistPad.  It will be much easier to edit in VSCode with this and the PowerShell extension.  You can add this from the Marketplace at this link

{% embed url="https://marketplace.visualstudio.com/items?itemName=vsls-contrib.gistfs" %}

You will need to sign into your GitHub account and authorize Visual Studio Code to access GitHub

![](<../../.gitbook/assets/image (176).png>)

## Getting the Gist Raw URL

You need to keep in mind that the URL shown for your Gist is one for the Gist Editor with your script in it, so you need to get the Raw URL by pressing the Raw button

```
Gist Editor URL Format
<Site>/<GitHubUser>/<Gist>

Gist Editor and Share URL
https://gist.github.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb
```

![](<../../.gitbook/assets/image (361).png>)

### Gist Raw Commit URL

After pressing the Raw button on the Gist Editor page, you are redirected to the Raw Commit page.  This URL for this page contains a commit string which means that it is static and the content will not reflect future changes that are made in the Gist Editor

![](<../../.gitbook/assets/image (210).png>)

```
Gist Raw Commit URL Format
<Site>/<GitHubUser>/<Gist>/raw/<Commit>/<ScriptName>

Gist Raw Commit URL
https://gist.githubusercontent.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb/raw/5b683d468aaaecc2640669b82833bfa23f7da5ea/Test-PowerShellGist.ps1
```

### Gist Raw URL (Latest)

To ensure you always have the latest content for your script, you need to remove the Commit string from the URL, so you will need to format your URL like this example. This is the URL you need to use for your PSCloudScript

```
Gist Raw URL Format
<Site>/<GitHubUser>/<Gist>/raw/<ScriptName>

Gist Raw URL
https://gist.githubusercontent.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb/raw/Test-PowerShellGist.ps1
```

![](<../../.gitbook/assets/image (320).png>)

### Get-GithubRawUrl

This function is part of the OSD PowerShell Module (version 22.1.15+) and it will return a GitHub Gist Raw URL for you automatically

```powershell
PS C:\> Get-GithubRawUrl https://gist.github.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb
https://gist.githubusercontent.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb/raw/Test-PowerShellGist.ps1
```

If your Gist has multiple files, those will be returned as well

```
PS C:\> Get-GithubRawUrl https://gist.github.com/MartinLuksik/a9902abc12aa831ef6719d15b5d24ac6
https://gist.githubusercontent.com/MartinLuksik/a9902abc12aa831ef6719d15b5d24ac6/raw/pwsh_ad_groups.ps1
https://gist.githubusercontent.com/MartinLuksik/a9902abc12aa831ef6719d15b5d24ac6/raw/pwsh_az_db_restore.ps1
https://gist.githubusercontent.com/MartinLuksik/a9902abc12aa831ef6719d15b5d24ac6/raw/pwsh_docker_container.md
https://gist.githubusercontent.com/MartinLuksik/a9902abc12aa831ef6719d15b5d24ac6/raw/pwsh_install_on_ubuntu.sh
https://gist.githubusercontent.com/MartinLuksik/a9902abc12aa831ef6719d15b5d24ac6/raw/pwsh_log_in_with_sp.ps1
```

{% hint style="info" %}
I could have started with this function, but I to make sure you understand how to get the proper URL
{% endhint %}

## Execute the Gist

Now that you have the proper Gist Raw URL, you can use execute the script using any of the following commands

```powershell
iex (irm https://gist.githubusercontent.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb/raw/Test-PowerShellGist.ps1)
irm https://gist.githubusercontent.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb/raw/Test-PowerShellGist.ps1 | iex

#Requires OSD Module 22.1.15+
irm (Get-GithubRawUrl https://gist.github.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb) | iex
iex (irm (Get-GithubRawUrl https://gist.github.com/OSDeploy/f43c96be58e09f20d1407c44a49721fb))
```

![](<../../.gitbook/assets/image (246).png>)

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

# 🆕 PSCloudScript Basics

I'm going to write a few posts about this topic, so it is probably best to start with the basics.  So what is a PSCloudScript?  It's what I call a PowerShell script that you run off the internet

[Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) gets mad props for supporting Community content like this. Make sure you check our their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

## GitHub gist

The first thing to do is to get your PowerShell script in the cloud, and you can do this easily by creating a GitHub Gist

{% embed url="https://gist.github.com" %}

It really doesn't matter if you save it as a Public or a Private gist as it's still accessable by anyone with the link

![](<../../../.gitbook/assets/image (216).png>)

## Visual Studio Code Extensions

There are several Visual Studio Code extensions that can help you manage your GitHub gists, but one that really stands out is GistPad.  It will be much easier to edit in VSCode with this and the PowerShell extension.  You can add this from the Marketplace at this link

{% embed url="https://marketplace.visualstudio.com/items?itemName=vsls-contrib.gistfs" %}

You will need to sign into your GitHub account and authorize Visual Studio Code to access GitHub

![](<../../../.gitbook/assets/image (176).png>)

This script that I'm working on will be used with OSDCloud.  Once you are done with all your script edits, save it back to GitHub gists

![](<../../../.gitbook/assets/image (189).png>)

## Getting the right URL

Its important to get the right Url to link for a PSCloudScript

### Gist Editor Share URL

&#x20;The GitHub Gist Editor and the Share link are identical, but the problem is that this URL doesn't have an easy way to get the PowerShell Script out

```
Gist Editor and Share URL Format
https://gist.github.com/<GitHubUser>/<Gist>

Gist Editor and Share URL
https://gist.github.com/OSDeploy/30dc8839e8972493be1a343d509f423f
```

![](<../../../.gitbook/assets/image (281).png>)

### Gist Raw Commit URL

When you press the Raw button on the Gist Editor Share Link, you are redirected to the Raw Commit Link

![](<../../../.gitbook/assets/image (244).png>)

You will get a URL returned that adds a second set of numbers.  This second set is known as the Commit, so if you share this URL, then you are sharing only this version of your script, so any future changes will not be reflected in this URL because this content will be static going forward

```
Gist Raw URL Format
<Site>/<GitHubUser>/<Gist>/raw/<Commit>/<ScriptName>

Gist Raw URL
https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/8fdf485273d0d69d6b54c624e758c9b90f6b5bce/live.osdcloud.com.ps1
```

### Gist Raw URL (Latest)

To ensure you always have the latest content for your script, you need to remove the Commit from the URL, so you will need to format your URL like this example. This is the URL you need to use for your PSCloudScript

```
Gist Raw URL Format
<Site>/<GitHubUser>/<Gist>/raw/<ScriptName>

Gist Raw URL
https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1
```

## WebRequest vs RestMethod

### Invoke-WebRequest

Once the Gist is published, and you have your real Gist Raw link, you could use the following command to get the Gist Content.  This command can be abbreviated as well

```powershell
$Uri = 'https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1'

(Invoke-WebRequest -UseBasicParsing -Uri $Uri).content
(iwr $Uri -UseB).content
```

![](<../../../.gitbook/assets/image (188).png>)

### Invoke-RestMethod

This is a better alternative for scripts as Invoke-RestMethod returns the content as structured data

```powershell
$Uri = 'https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1'

Invoke-RestMethod -Uri $Uri
irm $Uri
```

![](<../../../.gitbook/assets/image (288).png>)

## URL Shortner

The goal is to make the shorter, so why not try a URL shortener?  It is easy enough to test if the URL shortener you are going to use will work, and most of them should

{% embed url="https://www.shorturl.at" %}

![](<../../../.gitbook/assets/image (342).png>)

{% embed url="https://bitly.com" %}

![](<../../../.gitbook/assets/image (184).png>)

## Subdomains

This won't work with all hosts, but you may be able to redirect a subdomain if your DNS Host supports this.  I use [Google Domains](https://domains.google.com/registrar/) for OSDCloud.com and they make it very easy to do this

![](<../../../.gitbook/assets/image (193).png>)

This quickly allows me to get to my PSCloudScript by simply going to [live.osdcloud.com](http://live.osdcloud.com/)

One benefit of using GitHub Raw links is that the MIME information is presented as Text.  If you are linking to another source, you may be prompted to download the ps1 file rather than viewing the text

![](<../../../.gitbook/assets/image (229).png>)

Which looks really nice when I want to perform an `Invoke-RestMethod`

![](<../../../.gitbook/assets/image (241).png>)

## Executing a PSCloudScript

This is the simple part.  Once you have a method to get your PSCloudScript content, just use `Invoke-Expression -Command` to execute it.  Here are some examples on how to do this

```powershell
$Uri = 'https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1'

Invoke-Expression -Command (Invoke-RestMethod -Uri $Uri)
iex(irm $Uri)pow
Invoke-RestMethod -Uri $Uri | Invoke-Expression
irm $Uri|iex
```

![](<../../../.gitbook/assets/image (239).png>)

{% hint style="info" %}
You can test my script in full Windows as it will only execution the functions in WinPE
{% endhint %}

![](<../../../.gitbook/assets/image (314).png>)

## OSDCloud Live

So here I have a basic WinPE with PowerShell installed, but there are some issues running OSDCloud ...&#x20;

* PSGallery doesn't work
* Curl.exe isn't present
* OSD Module isn't installed

![](<../../../.gitbook/assets/image (168).png>)

So instead of mounting and making changes to the WIM, I'll script my solution and run this as a PSCloudScript.  From a Command Prompt, any of these commands will work

```batch
REM Too long
powershell Invoke-Expression -Command (Invoke-RestMethod -Uri live.osdcloud.com)

REM Still too long
powershell Invoke-RestMethod -Uri live.osdcloud.com | Invoke-Expression

REM Not everyone will understand the pipe
powershell irm live.osdcloud.com | iex

REM Shortest command line, but will non-ps users understand a pipe?
powershell irm live.osdcloud.com|iex

REM Ideal command line
powershell iex(irm live.osdcloud.com)
```

Optionally these can be run if you are already in PowerShell

```powershell
#Too long
Invoke-Expression -Command (Invoke-RestMethod -Uri live.osdcloud.com)

#Still too long
Invoke-RestMethod -Uri live.osdcloud.com | Invoke-Expression

#Not everyone will understand the pipe
irm live.osdcloud.com | iex

#Shortest command line, but will non-ps users understand a pipe?
irm live.osdcloud.com|iex

#Ideal command line
iex(irm live.osdcloud.com)
```

Now I can boot to WinPE and run OSDCloud as long as it has PowerShell from the ADK installed :)

![](<../../../.gitbook/assets/image (326).png>)

---
description: 22.1.11
---

# 🆕 OSD January Update

I've enjoyed taking some time off OSD to catch up on things going on in the office, and at home, but I have quietly been working on some updates for OSD and I thought I would share them.  Keep in mind these changes will not be released until after Microsoft publishes today's Patch Tuesday updates.

## PowerShell Module Updates

* **OSD 22.1.11.1**
* **OSDSUS 22.1.11**

### New Functions

* **Get-GithubRawUrl**
* **Get-GitHubRawContent**
* **Resolve-MsUrl**

## OSDCloud Azure Blob SAS Images

This update includes support for an Azure Blob SAS Url as the ImageFile thanks to some modifications from [Iain Brighton](https://twitter.com/iainbrighton)

![](<../../../.gitbook/assets/image (308).png>)

![](<../../../.gitbook/assets/image (277).png>)

The downside of this is you now have a long URL that you won't remember, so you will probably want to put this in a script.  The really really bad news is that this will be in plain text and that isn't a good thing.  I'll address in an upcoming post&#x20;

## Get-GithubRawUrl

This simple function is performing a simple conversion of the URL provided and returning the RAW link.  One cool thing about this is that it will return multiple URL's when linked to a Gist with multip scripts

![](<../../../.gitbook/assets/image (317).png>)

## Get-GithubRawContent

This function takes a URL and returns the RAW content (using the `Get-GithubRawUrl` function)

![](<../../../.gitbook/assets/image (312).png>)

## Resolve-MsUrl

This new function will resolve Microsoft aka.ms or fwlink URL's

![](<../../../.gitbook/assets/image (211).png>)

It's actually quite a simple function you can easily do on your own

```powershell
PS C:\> (Invoke-WebRequest 'https://go.microsoft.com/fwlink/p/?LinkID=2182910' -UseBasicParsing -Method Head -MaximumRedirection 0 -ErrorAction SilentlyContinue).Headers.Location
https://oneclient.sfx.ms/Win/Prod/21.230.1107.0004/OneDriveSetup.exe
```


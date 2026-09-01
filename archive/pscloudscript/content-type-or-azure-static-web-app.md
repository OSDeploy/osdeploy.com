---
description: 22.1.16
---

# Content-Type | Azure Static Web App

So now is about the time that I address what Content-Type is and how it relates to hosting PowerShell files on the internet.  To demonstrate this, I'll create an Azure Static Web App

## Azure Static Web App

Azure Static Web App is a service that can publish a GitHub Repo to an Azure Web using GitHub Actions.  You can learn more at the following links

{% embed url="https://azure.microsoft.com/en-us/services/app-service/static" %}

{% embed url="https://github.com/features/actions" %}

### Configuration

The configuration is very simple, I used defaults for most settings

![](<../../.gitbook/assets/image (225).png>)

### GitHub Actions

The first time I ran this it failed because I didn't have an Index.html in the root of my GitHub Repo, so make sure you check the Job for any errors

![](<../../.gitbook/assets/image (295).png>)

So I simply created a copy of a PowerShell script and saved it as Index.html in the root of my Repo and had the Build and Deploy Job run again

![](<../../.gitbook/assets/image (185).png>)

## Browser Test

Once the Azure Static Web App build finally completed, my PSCloudScript GitHub Repo was published at the following link

{% embed url="https://agreeable-river-0b8a1af10.1.azurestaticapps.net" %}

Opening this in Chrome shows all of the PowerShell formatting is lost

![](<../../.gitbook/assets/image (162).png>)

Additionally, when I navigate to the URL of one of my PowerShell .ps1 files, I'm prompted to download the file instead of viewing the content in Chrome.  This is not the same results that I received when viewing the content on GitHub's Raw links

```
https://agreeable-river-0b8a1af10.1.azurestaticapps.net/Test-PSCloudScriptAzStaticWebApp.ps1
```

The reason why the PowerShell script saved as HTML, and the .ps1 file don't render the same as they do in GitHub Raw is because of the Content-Type.  A quick check of the different URL's with `Invoke-WebRequest` confirms this.  These files are absolutely being rendered based on their Content-Type

```powershell
#Index.html
PS C:\> (Invoke-WebRequest -UseBasicParsing agreeable-river-0b8a1af10.1.azurestaticapps.net).Headers.'Content-Type'
text/html

#Test-PSCloudScriptAzStaticWebApp.ps1
PS C:\> (Invoke-WebRequest -UseBasicParsing agreeable-river-0b8a1af10.1.azurestaticapps.net/Test-PSCloudScriptAzStaticWebApp.ps1).Headers.'Content-Type'
application/octet-stream

#GitHub Raw Test-PSCloudScriptAzStaticWebApp.ps1
PS C:\> (Invoke-WebRequest -UseBasicParsing https://raw.githubusercontent.com/OSDeploy/PSCloudScript/main/Test-PSCloudScriptAzStaticWebApp.ps1).Headers.'Content-Type'
text/plain; charset=utf-8
```

## Azure Static Web App Config

To keep the PowerShell formatting in the .html and the .ps1 file, it is necessary to change the Content-Type to **`text/plain`** (the same as GitHub Raw).  To change this on an Azure Static Web App, I can simply create a file called **`staticwebapp.config.json`** in the root of my GitHub Repo with the following settings

{% code title="staticwebapp.config.json" %}
```json
{
    "mimeTypes": {
        ".htm": "text/plain",
        ".html": "text/plain",
        ".json": "text/json",
        ".md": "text/markdown",
        ".psm1": "text/plain",
        ".ps1": "text/plain"
    }
}
```
{% endcode %}

This configuration change is detailed in Microsoft Docs at the following link

{% embed url="https://docs.microsoft.com/en-us/azure/static-web-apps/configuration" %}

After creating the config file and pushing it to GitHub, the GitHub Action will publish the content back to my Azure Static Web.  Another test in Chrome shows the `text/plain` formatting in index.html, and the PowerShell file will now display in the browser rather than prompting for a download

![](<../../.gitbook/assets/image (268).png>)

![](<../../.gitbook/assets/image (217).png>)

## Summary

If you are publishing PowerShell scripts to a Web Site, make sure you set the Content-Type for the files to **`text/plain`** if you want them displayed and rendered properly in a browser.  Additionally, if you are publishing PowerShell scripts to an **Azure Blob Storage**, you can set the **Content-Type** to **`text/plain`** as needed using **Azure Storage Explorer**

{% embed url="https://azure.microsoft.com/en-us/features/storage-explorer" %}

![](<../../.gitbook/assets/image (199).png>)

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

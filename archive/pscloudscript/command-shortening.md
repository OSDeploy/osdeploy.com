---
description: 22.1.16
---

# Command Shortening

Ok, so now you have your PowerShell file in the cloud, but you have a 156 character command, how can this be shortened?

```powershell
Invoke-Expression -Command (Invoke-RestMethod -Uri 'https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1')
```



## Alias

Start by using the PowerShell alias iex to replace Invoke-Expression, and irm instead of Invoke-RestMethod.  Now you are down to 128 characters

```powershell
iex -Command (irm -Uri 'https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1')
```

## Parameter Names

You can leave off the mandatory parameter names for iex and irm to trim down to 114 characters

```powershell
iex (irm 'https://gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1')
```

## Splitting Hairs

Finally, you can lose the quotes around the URL if you don't have any special characters and possibly https://, but that won't recover much.  You can also change replace the parenthesis with a pipe to iex, which gets you to 104, but that's still too long

```powershell
irm gist.githubusercontent.com/OSDeploy/30dc8839e8972493be1a343d509f423f/raw/live.osdcloud.com.ps1 | iex
```

## URL Shortener

Another option is to use a URL Shortener service, but make sure you test it first as there are no guarantees this will work.  The following two have been tested and work fine

### Bitly

{% embed url="https://bitly.com" %}

![](<../../.gitbook/assets/image (184).png>)

### ShortURL

{% embed url="https://www.shorturl.at" %}

![](<../../.gitbook/assets/image (342).png>)

## Domain Forwarding

Another option you can try is to do Domain Forwarding.  This won't work with all hosts, but you may be able to redirect a subdomain if your DNS Host supports this.  I use [Google Domains](https://domains.google.com/registrar/) for [OSDCloud.com](https://osdcloud.osdeploy.com/) and they make it very easy to do this in their Admin Console

![](<../../.gitbook/assets/image (193).png>)

This quickly allows me to get to my PSCloudScript by simply going to [live.osdcloud.com](http://live.osdcloud.com/)

One benefit of using GitHub Raw links is that the MIME information is presented as Text.  If you are linking to another source, you may be prompted to download the ps1 file rather than viewing the text

![](<../../.gitbook/assets/image (229).png>)

This makes it very easy to use in **OSDCloud**

![](<../../.gitbook/assets/image (326).png>)

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

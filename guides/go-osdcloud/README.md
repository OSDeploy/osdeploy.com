---
description: Azure Functions Go Url Shortener
---

# go OSDCloud

I'm going to quickly detail how to make a quick and cheap URL Shortener in an Azure Function App.  The goal is to use `go.osdcloud.com` subdomain to run this function.  I strongly recommend reviewing the documentation at the following link

{% embed url="https://docs.microsoft.com/en-us/azure/azure-functions" %}

While this uses an Azure Function App, the redirect will use proxies.json, without using an API.  It's important to note that this feature is only supported in Azure Functions 1.x - 3.x, and it is not supported in the current GA 4.x.  My guess is that Microsoft wants to force you to subscribe to Azure API Management which is substantially more expensive (around $50 / month). You can read about Azure Functions Proxies here

{% embed url="https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies" %}

That said, there are a few main steps involved in getting the URL Shortener to work

{% content-ref url="azure-function.md" %}
[azure-function.md](azure-function.md)
{% endcontent-ref %}

{% content-ref url="custom-domain.md" %}
[custom-domain.md](custom-domain.md)
{% endcontent-ref %}

{% content-ref url="ssl-binding.md" %}
[ssl-binding.md](ssl-binding.md)
{% endcontent-ref %}

{% content-ref url="proxies.md" %}
[proxies.md](proxies.md)
{% endcontent-ref %}

## Sponsor

**OSDeploy is sponsored by** [**Recast Software**](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) **and their Systems Management Tools**

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

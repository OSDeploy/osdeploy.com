---
description: Azure Function App
---

# SSL Binding

## Open Private Key Certificate

Now that you have added your Custom Domain, you will need to create an SSL Certificate for proper HTTPS redirection.  Complete the following steps

1. **TLS/SSL settings**
2. **Private Key Certificates (.pfx)**
3. **Create App Service Managed Certificate**

![](<../../.gitbook/assets/image (363).png>)

## Create Private Key Certificate (.pfx)

* **Select your subdomain from the combo box**
* **Press Create**

![](<../../.gitbook/assets/image (245).png>)

![](<../../.gitbook/assets/image (196).png>)

## Add TLS/SSL Binding

1. **Select Custom domains**
2. **Add binding**
3. **Select your Custom domain from the combo box**
4. **Select your Private Certificate from the combo box**
5. **Select the TLS/SSL type from the combo box**
6. **Press Add Binding**

![](<../../.gitbook/assets/image (206).png>)

## Set HTTPS Only

1. **Custom domain should have an SSL Binding**
2. **Toggle HTTPS Only: On**

![](<../../.gitbook/assets/image (249).png>)

## Test Custom Domain

Give it a quick test with and without HTTPS

![](<../../.gitbook/assets/image (359).png>)

## Sponsor

**OSDeploy is sponsored by** [**Recast Software**](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) **and their Systems Management Tools**

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

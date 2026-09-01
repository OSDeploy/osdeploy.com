# Proxies

If you have made it this far, then you are ready to create your Proxies.  Here are some examples

## any (Catch All)

At an absolute minimum, you should make a Catch All.  This is used to redirect any traffic that does not match a route.  Redirect to a Help page or to your Home page, either is a good choice

1. **Proxies**
2. **Add**
3. **Name: any**
4. **Route template: /{\*any}**
5. **Response override**
6. **Status code: 302**
7. **Status message: Temporary Redirect**
8. **Headers Name: Location**
9. **Headers Value: \<Home or Help page>**
10. **Create**

![](<../../.gitbook/assets/image (316).png>)

{% code title="proxies.json" %}
```json
{
  "$schema": "http://json.schemastore.org/proxies",
  "proxies": {
    "any": {
      "matchCondition": {
        "route": "/{*any}"
      },
      "responseOverrides": {
        "response.statusCode": "302",
        "response.statusReason": "Temporary Redirect",
        "response.headers.Location": "https://osdcloud.com"
      }
    }
  }
}
```
{% endcode %}

## twitter

Here's an example of [https://go.osdcloud.com/twitter](https://go.osdcloud.com/twitter)

1. **Proxies**
2. **Add**
3. **Name: any**
4. **Route template: /twitter**
5. **Response override**
6. **Status code: 302**
7. **Status message: Temporary Redirect**
8. **Headers Name: Location**
9. **Headers Value: \<your Twitter profile>**
10. **Create**

![](<../../.gitbook/assets/image (238).png>)

{% code title="proxies.json" %}
```json
{
  "$schema": "http://json.schemastore.org/proxies",
  "proxies": {
    "any": {
      "matchCondition": {
        "route": "/{*any}"
      },
      "responseOverrides": {
        "response.statusCode": "302",
        "response.statusReason": "Temporary Redirect",
        "response.headers.Location": "https://osdcloud.com"
      }
    },
    "twitter": {
      "matchCondition": {
        "route": "/twitter"
      },
      "responseOverrides": {
        "response.statusCode": "302",
        "response.statusReason": "Temporary Redirect",
        "response.headers.Location": "https://twitter.com/SeguraOSD"
      }
    }
  }
}
```
{% endcode %}

## Editing proxies.json

If you are good with editing JSON files directly, you can absolutely do this in the Azure Portal

1. App Files
2. Select proxies.json from the combo box

![](<../../.gitbook/assets/image (215).png>)

## Sponsor

**OSDeploy is sponsored by** [**Recast Software**](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) **and their Systems Management Tools**

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

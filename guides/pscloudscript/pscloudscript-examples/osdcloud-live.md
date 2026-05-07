# OSDCloud Live

OSDCloud Live is a method of starting OSDCloud from an ADK WinPE with the PowerShell package added.  Normal WinPE will boot to a command prompt and start `wpeinit`.  From here, enter the following command

```
powershell iex (irm live.osdcloud.com)
```

![](<../../../.gitbook/assets/image (287).png>)

This command will use `Invoke-RestMethod` to return the content of the PowerShell script into a string, and then use `Invoke-Expression` to run the string

![](<../../../.gitbook/assets/image (257).png>)

You can review the PowerShell script from the GitHub Gist at the following link

{% embed url="https://gist.github.com/OSDeploy/30dc8839e8972493be1a343d509f423f" %}

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

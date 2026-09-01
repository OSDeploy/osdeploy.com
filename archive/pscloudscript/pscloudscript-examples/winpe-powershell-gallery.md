# WinPE PowerShell Gallery

This example will enable PowerShell Gallery in an ADK WinPE with the PowerShell package added.  Normal WinPE will boot to a command prompt and start `wpeinit`.  From here, enter the following command

```
powershell iex(irm psgallery.ps1.osdeploy.com)
```

This command will use `Invoke-RestMethod` to return the content of the PowerShell script into a string, and then use `Invoke-Expression` to run the string

![](<../../../.gitbook/assets/image (275).png>)

## PSCloudScript

psgallery.ps1.osdeploy.com will redirect to the following script

{% embed url="https://github.com/OSDeploy/ps1.osdeploy.com/blob/main/psgallery.ps1" %}

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

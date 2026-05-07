# OSDCloud WinPE and OOBE

In this example, I configured a PSCloudScript that had separate routines for WinPE and OOBE.  Both routines used the same command line

```
powershell iex(irm 10.demo.osdcloud.com)
```

{% hint style="warning" %}
The above URL is broken at this time until my testing is complete
{% endhint %}

## WinPE

This routine had OSDCloud preconfigured with everything except the OSLanguage.  I work for a global company, so a single language won't work

```powershell
Start-OSDCloud -OSBuild 20H2 -OSEdition Enterprise -OSLicense Volume -Firmware -SkipAutopilot -SkipODT -Restart
```

This allows the technician to specify the Language during deployment.  This deployment was configured to Restart to OOBE automatically

![](<../../../.gitbook/assets/image (348).png>)

![](<../../../.gitbook/assets/image (161).png>)

![](<../../../.gitbook/assets/image (365).png>)

![](<../../../.gitbook/assets/image (228).png>)

## OOBE

Press Shift + F10 to open a command prompt from this screen

![](<../../../.gitbook/assets/image (280).png>)

This OOBE is configured to open Display Settings so I could resize the screen as needed on my Virtual Machine.  Closing Display Settings will continue to the next step

![](<../../../.gitbook/assets/image (166).png>)

Language is important as it gives me the ability to add an additional language or keyboard if necessary

![](<../../../.gitbook/assets/image (203).png>)

Finally Date and Time Settings so I can change from Pacific Time Zone to something a little closer to home

![](<../../../.gitbook/assets/image (325).png>)

Autopilot was configured with my GroupTag in the script, so all I needed to do was enter my credentials to join my Tenant.  You will notice the main window was configured to remove specific Appx Packages and to add Windows Capabilities that I needed like RSAT and NetFX3

![](<../../../.gitbook/assets/image (279).png>)

Windows Update was also enabled for Drivers and the Operating System

![](<../../../.gitbook/assets/image (272).png>)

Finally, the process ends with an automatic reboot that will wait for the Autopilot to complete first&#x20;

![](<../../../.gitbook/assets/image (223).png>)

All the above OOBE steps were completed with this simple configuration

```powershell
#=================================================
#   oobeCloud Settings
#=================================================
$Global:oobeCloud = @{
    oobeSetDisplay = $true
    oobeSetRegionLanguage = $true
    oobeSetDateTime = $true
    oobeRegisterAutopilot = $true
    oobeRegisterAutopilotCommand = 'Get-WindowsAutopilotInfo -Online -GroupTag Demo -Assign'
    oobeRemoveAppxPackage = $true
    oobeRemoveAppxPackageName = 'CommunicationsApps','OfficeHub','People','Skype','Solitaire','Xbox','ZuneMusic','ZuneVideo'
    oobeAddCapability = $true
    oobeAddCapabilityName = 'ActiveDirectory','BitLocker','GroupPolicy','RemoteDesktop','ServerManager','VolumeActivation','NetFX'
    oobeUpdateDrivers = $true
    oobeUpdateWindows = $true
    oobeRestartComputer = $true
    oobeStopComputer = $false
}
```



## Summary

This is an incredibly easy way to deploy an Autopilot device without any infrastructure with a single command line used in WinPE and OOBE.  I'm still working on some cleanup, but expect to hear more soon

```
powershell iex(irm 10.demo.osdcloud.com)
```

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

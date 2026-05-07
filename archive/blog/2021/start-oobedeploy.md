---
description: 21.7.26
---

# Start-OOBEDeploy

This new function is part of the OSD PowerShell Module and is meant to "bridge" an OEM Image to Enterprise Ready. It can be used in conjunction with AutopilotOOBE, or standalone

{% hint style="warning" %}
This function is experimental for now so things may change, and this document will change too :)
{% endhint %}

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
OSDeploy is Sponsored by Recast Software
{% endembed %}

## OOBE

Also known as Out of Box Experience, this is where you will use this function as designed.  It doesn't matter if this device is Autopilot Enrolled or not at this point.  The important thing to remember is that in this state, your system can get to anything on the Internet, including Windows Update :)

![](<../../../.gitbook/assets/image (439).png>)

### Command Prompt

Start by pressing Shift + F10 to open a Command Prompt.  If you are on a laptop keyboard, you may have to add the Fn key.  Once that is open, simply Start PowerShell.  You can close the Command Prompt at this point

![](<../../../.gitbook/assets/image (434).png>)

### PowerShell

Run the following lines in PowerShell to make sure you have the latest OSD PowerShell Module from the PowerShell Gallery

```
Set-ExecutionPolicy RemoteSigned -Force
Install-Module OSD -Force
Import-Module OSD -Force
```

If you are on a VM like me, you can use the following command to change your resolution

```
Set-DisRes 1600
```

![](<../../../.gitbook/assets/image (378).png>)

## Start-OOBEDeploy

Let's take a look at Start-OOBEDeploy now without any parameters.  As you can see the only thing it really does is&#x20;

* Write a Transcript to C:\Windows\Temp
* Set PSGallery as Trusted

![](<../../../.gitbook/assets/image (379).png>)

## -AddNetFX3

You can easily enable any Windows Capability since you aren't blocked by any WSUS policy

![](<../../../.gitbook/assets/image (374).png>)

![](<../../../.gitbook/assets/image (397).png>)

## -AddRSAT

This is now a breeze to install your Admin Tools before you even get logged into Windows

![](<../../../.gitbook/assets/image (401).png>)

## -SetEdition Enterprise

We deploy Windows 10 Enterprise, but our new computers from Dell and Lenovo are Windows 10 Pro.  This parameter takes care of all of that

![](<../../../.gitbook/assets/image (404).png>)

## -RemoveAppx

I don't know about you, but I don't like all the default shit that comes in Windows 10, so I like to nip that in the bud

![](<../../../.gitbook/assets/image (425).png>)

I simply give some strings to match and let it take care of the rest

![](<../../../.gitbook/assets/image (377).png>)

Much better!

![](<../../../.gitbook/assets/image (386).png>)

## -UpdateDrivers

{% hint style="info" %}
This will install the PSWindowsUpdate Module to do the heavy lifting
{% endhint %}

This will update all your installed Drivers from Windows Update.  Sorry no screenshots as there is nothing to update in Hyper-V

## -UpdateWindows

{% hint style="info" %}
This will install the PSWindowsUpdate Module to do the heavy lifting
{% endhint %}

## -CustomProfile

If you want to automate all of your settings into a Custom Profile, let me know what you want and I'll add it to the Module (AutopilotOOBE uses CustomProfile as well!)

![](<../../../.gitbook/assets/image (392).png>)

![](<../../../.gitbook/assets/image (410).png>)

![](<../../../.gitbook/assets/image (384).png>)

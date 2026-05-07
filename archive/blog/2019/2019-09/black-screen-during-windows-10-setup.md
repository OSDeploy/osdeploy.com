# Black Screen During Windows 10 Setup

## The Symptoms

#### Lots of different examples of the issue.  Its most noticeable when replacing the OneDriveSetup.exe with a newer version, but this is not the cause of the problem

{% embed url="https://twitter.com/DlHerve/status/1167402064667979776" %}

{% embed url="https://twitter.com/crock23a/status/1172589981392658437" %}

{% embed url="https://twitter.com/SuneThomsenDK/status/1168639789266083840" %}

{% embed url="https://twitter.com/SeguraOSD/status/1170042930633068544" %}

{% embed url="https://twitter.com/JrocketB/status/1170043423547711490" %}

## The REAL Cause

The real problem has nothing to do with OneDrive being modified or updated.  The real cause of the problem is a bad Intel IASTOR Driver.  In the Dell Latitude 5400 that I was testing in, it used Intel 17.2.6.1027 from March 19, 2019

## Workaround - Change SATA Operating from RAID to AHCI

You can change the SATA Operation from RAID to AHCI.  The reason this works is because this changes the HardwareID of the SATA Controller, thus using a different Driver entirely.  **Don't ask me to prove this for you ... I already know this as fact and it should be common knowledge by now.**  If you have doubts then image a Computer in RAID and get the HardwareID of the SATA Controller.  Then change it to AHCI in the BIOS and reimage the Computer.  You will see that you have a completely different HardwareID, and a different Driver INF

{% hint style="danger" %}
**This is NOT a solution, so really, don't use this as the fix.  You'll probably find that future Windows 10 Upgrades will be blocked by this issue, not to mention the MANY future issues that may occur due to this crap Driver**
{% endhint %}

{% embed url="https://twitter.com/SeguraOSD/status/1170044593150337026" %}

## The REAL Solution

I've already detailed that the problem is related to the Intel IASTOR Driver, but apparently with only 4 likes, lots of doubters, so here I am 1:00 AM Saturday having to prove why this is the solution

{% embed url="https://twitter.com/SeguraOSD/status/1170051727762100226" %}

## FAIL Test Process

* **Dell Latitude 5400**
* **Windows 10 1809 fully updated with** [**OSDBuilder**](http://osdbuilder.osdeploy.com)
* **Dell Model Pack 5400-win10-A02-T5G5M.CAB using** [**OSDDrivers**](http://osddrivers.osdeploy.com)
* **USB Deployment using Windows Setup to rule out ConfigMgr and MDT**

#### As expected, this results in a black screen during Windows Setup.  Attached is my setupact.log

{% file src="../../../../.gitbook/assets/setupact.log" %}

#### What everybody seems to be focused with is the HASH mismatch for OneDriveSetup.exe, while completely ignoring the ERRORS (the RED lines)

![](<../../../../.gitbook/assets/image (155).png>)

## SUCCESS Test Process

Not much else we can do with this log without having something to compare it to, so let's test what I already know is the solution.

* **Dell Latitude 5400**
* **Windows 10 1809 fully updated with** [**OSDBuilder**](http://osdbuilder.osdeploy.com)
* **Dell Model Pack 5400-win10-A02-T5G5M.CAB using** [**OSDDrivers**](http://osddrivers.osdeploy.com)
* **USB Deployment using Windows Setup to rule out ConfigMgr and MDT**
* **Updated** [**Intel IASTOR Driver 17.5.2.1024**](https://www.catalog.update.microsoft.com/Search.aspx?q=PCI%5cVEN_8086%26DEV_282A) **in $WinPEDriver$**

{% file src="../../../../.gitbook/assets/setupact (1).log" %}

### $WinPEDriver$

Not too many people know about this method, but if you are running Windows Setup from Media, you can add a $WinPEDriver$ directory and add Storage and Network Drivers.  These are added to WinSE (Windows Setup Environment) and also added to Windows 10

![](<../../../../.gitbook/assets/image (146).png>)

If you want learn more about this, read the following link

{% embed url="https://support.microsoft.com/en-us/help/2686316/limitations-of-winpedriver-when-used-in-conjunction-with-other-driver" %}

You can see this being added to WinSE in the SetupAct.log

![](<../../../../.gitbook/assets/image (145).png>)

Here is what it looks like adding the Driver to Windows 10 in Offline Servicing

![](<../../../../.gitbook/assets/image (154).png>)

#### Guess what?  No black screen!

## SetupAct SUCCESS Driver Installation

This is normally what appears in the Driver installation process

![](<../../../../.gitbook/assets/image (151).png>)

## SetupAct FAIL Driver Installation

In the failure, it seems that a Driver has thrown a CBS Check

![](<../../../../.gitbook/assets/image (149).png>)

## SetupAct SUCCESS First Boot

This is what the First Boot section of SetupAct should look like

![](<../../../../.gitbook/assets/image (152).png>)

## SetupAct FAIL First Boot

The difference between the success and the failure is incredible.  In the failure, you can see **"Failed to get Reserve Manager"**.  It is at this point where CBS checks for file errors, which causes the delay.

![](<../../../../.gitbook/assets/image (148).png>)

## Conclusion

#### It's not OneDriveSetup.exe ... it's the Intel Driver.  Anything short of replacing the Driver is just a workaround or a shortcut

On some computers the black screen will be present for an incredible amount of time, over an hour in some cases.  Here's why.  The OptiPlex will take much longer as this is using a spinning disk and not an SSD or NVMe Drive.  Additionally, if this is an OS Upgrade, you will have to wait for the CBS to process much more OS Data.

## Why does OneDriveSetup.exe Choke?

Consider OneDriveSetup.exe as the "Canary in the Coal Mine".  Its no coincidence that IASTOR is low level, and OneDrive has its own low level file operations.

## So why not just leave OneDriveSetup.exe alone?

You can, but consider the possibility that the newer OneDrive has more internal features, like Known Folder Moves ... and also consider that OneDrive will get updated automatically to the newer version, so while you won't have an issue during OS Deployment, you may get the black screen issue after OneDrive has been updated.

## One Last Thing

#### Take the time to learn how to read the LOG files.  You will save yourself quite a bit of headache.  There were lots of RED FLAGS that were in there ... but missed.

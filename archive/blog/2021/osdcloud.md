---
description: March 10, 2021
---

# OSDCloud

## What is OSDCloud?

Its a way to image a computer or Virtual Machine by booting to WinPE and pulling everything from the Cloud ... that's it.  Windows 10 will require 4GB and Drivers will consume between 1-2GB

## Requirements

* WinPE
  * PowerShell support (from ADK)
  * cURL (from Windows 10 1803+)
  * PowerShell Gallery
  * Tested on WinPE from Windows 10 20H2
* Supported Hardware (for now)
  * Dell
  * Microsoft Hyper-V
* Boot Device
  * USB for Physical
  * ISO for Virtual
* Ethernet
* Internet
  * Open
  * Untested with Proxy or Firewall
* PowerShell Modules
  * OSD
  * OSDSUS
  * PackageManagement
  * PowerShellGet
* GitHub Repository

## WinPE

Hopefully you know how to create WinPE, you can use your existing one if you must, or you can easily create one with OSDBuilder 21.3.9+

### PowerShell

You can add this from the ADK.  If you are not sure how to do this, then OSDCloud may be a bit out of reach for now

### cURL Support

Starting with Windows 10 1803, cURL.exe and Tar.exe were included.  These need to be injected into your WinPE wim at Windows\System32.  If you are using OSDBuilder 21.3.9+, then these files are copied automatically into an AutoExtraFiles directory

![](<../../../.gitbook/assets/image (259).png>)

You can add these to WinPE by creating a New-PEBuildTask and adding the **`-WinPEAutoExtraFiles`** parameter

```
New-PEBuildTask -SourceWim WinRE -TaskName OSDCloud -CustomName OSDCloud -WinPEAutoExtraFiles
```

### PowerShell Gallery support

If you have the OSD PowerShell Module 21.3.9+ installed, then you can enable it by reading this link

{% embed url="https://osd.osdeploy.com/module/functions/winpe/enable-pewimpsgallery" %}

And that's it.  While you can add the OSD PowerShell Module to WinPE, you can also do this in WinPE since you will have the PowerShell Gallery working, and you get the latest version

## Boot to WinPE

So hopefully you have WinPE working, so make sure it is connected to the Internet and give it a boot

### Install-Module OSD

Now you will need to install the OSD PowerShell Module as much of the dependencies are in here.  This is no problem since you have a working PowerShell Gallery!

![](<../../../.gitbook/assets/image (347).png>)

![](<../../../.gitbook/assets/image (340).png>)

## Start-OSDCloud

By default, **`Start-OSCloud`** will launch in my OSDCloud Repository.  Feel free to Clone or make your own, but you can just use mine for testing

{% hint style="danger" %}
My OSDCloud Repository should only be used as an example on how to configure one.  I am still working on adding some things, so things may change at any time and I am not responsible for you messing your stuff up, so there's that!
{% endhint %}

{% embed url="https://github.com/OSDeploy/OSDCloud" %}

You have some parameters you can use with **`Start-OSDCloud`** to get you going.  Let's say your GitHub UserName is SlackDaddy2021.  If so, this would be your command

```
Start-OSDCloud -GitHubUser 'SlackDaddy2021'
```

Here is the details ... its really just a simple script

![](<../../../.gitbook/assets/image (167).png>)

![](<../../../.gitbook/assets/image (351).png>)

Now press Enter and you should see a crude menu

{% hint style="warning" %}
If you don't get the menu, and get some PowerShell errors, its more than likely you don't have an open Internet connection
{% endhint %}

I have things broken down to test individual parts of my script, but I recommend you type ENT (or EDU or PRO) and press Enter

![](<../../../.gitbook/assets/image (183).png>)

### Disk Preparation

After enabling the High Performance Power Plan, you will be prompted to Clear-LocalDisk and create a New-OSDisk.  All 3 of these steps are completed with functions from the OSD Module

![](<../../../.gitbook/assets/image (221).png>)

![](<../../../.gitbook/assets/image (251).png>)

### Update-MyDellBIOS

Well this isn't working for now, but it does download to X:\Windows\Temp ...&#x20;

![](<../../../.gitbook/assets/image (156).png>)

### Download Windows 10 ESD

To find the download URL's for Windows 10, the script will install the **`OSDSUS`** PowerShell Module and do a query for the links.  You can obviously configure this to do whatever Edition and Language that you need in your own GitHub Repository.  cURL will be used to download the ESD to C:\OSDCloud\ESD

![](<../../../.gitbook/assets/image (290).png>)

### Expand Windows 10 ESD

Once the ESD has downloaded, the proper Index will be expanded to C:\ and the System Partition will be prepared

![](<../../../.gitbook/assets/image (358).png>)

### Save-MyDellDriverCab

This is another function in the OSD PowerShell Module.  It will match the proper Dell Driver Pack for your computer and download it using cURL to C:\Drivers and it will be expanded

![](<../../../.gitbook/assets/image (197).png>)

![](<../../../.gitbook/assets/image (333).png>)

### Apply Drivers using Unattend offlineServicing

OSDCloud will drop an Unattend.xml and then apply it to add the Drivers to the offline Windows 10.  This step will take some time.  When complete, you can validate the oem Drivers in C:\Windows\Inf

```
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="offlineServicing">
        <component name="Microsoft-Windows-PnpCustomizationsNonWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <DriverPaths>
                <PathAndCredentials wcm:keyValue="1" wcm:action="add">
                    <Path>C:\Drivers</Path>
                </PathAndCredentials>
            </DriverPaths>
        </component>
    </settings>
</unattend>
```

![](<../../../.gitbook/assets/image (235).png>)

### Complete

After the Drivers have been applied, the WinPE phase is complete.  You can reboot and proceed to OOBE

![](<../../../.gitbook/assets/image (303).png>)

## Time = 6 Minutes

I made an effort to not mention anything about how long it takes to complete OSDCloud, but if you look at the screenshots above, you will see that the Start Time is shown for each section.  I am running this from home with a 600MB connection, so YMMV on this one.  The entire process took about 6 Minutes, with 1 Minute to download the ESD, less than 1 Minute to download and expand the Dell Driver Pack, and 2.5 Minutes to apply the Drivers offline

```
23:43:18	Start
23:43:55	Download Windows 10 ESD (4GB)
23:44:54	Expand Windows 10 ESD
23:46:07	Download and Expand Dell Drivers (1.5GB)
23:46:56	Apply Drivers
23:49:29	Complete
```

## Next Steps

I'll keep working on the script.  My next steps are to use Save-Module to add all the AutoPilot stuff into the OS.  Its also possible to inject an AutoPilotConfiguration.json, but that will come in the next day or two.

Your next step is to make a WinPE with PowerShell and PowerShell Gallery support!

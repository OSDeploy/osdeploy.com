# WindowsCapability -and WindowsPackage

{% hint style="warning" %}
You will need to install the OSD Module version 21.2.8.2 or higher
{% endhint %}

Windows Capabilities and Windows Packages have extremely long names.  Most of us have gotten quite good and guessing what they are

To get a list of all Windows Packages that are Installed (or Superseded) on your computer, simply run the following command in PowerShell

```
(Get-WindowsPackage -Online).PackageName
(Get-WindowsCapability -Online).Name
```

Here are some examples that were returned on my system

```
Microsoft-Windows-Client-LanguagePack-Package~31bf3856ad364e35~amd64~en-US~10.0.19041.746
Microsoft-Windows-LanguageFeatures-Handwriting-en-us-Package~31bf3856ad364e35~amd64~~10.0.19041.1
Microsoft-Windows-TabletPCMath-Package~31bf3856ad364e35~amd64~~10.0.19041.746
Microsoft-Windows-WordPad-FoD-Package~31bf3856ad364e35~amd64~en-US~10.0.19041.1
Package_for_RollupFix~31bf3856ad364e35~amd64~~19041.685.1.6
```

## Tilde Delimitator

While the long name may be a challenge to read as a complete string, you can separate each of the Windows Packages or Windows Capabilities with a Tilde.  Like this:

```
"Microsoft-Windows-Client-LanguagePack-Package~31bf3856ad364e35~amd64~en-US~10.0.19041.746" -split "~"

Microsoft-Windows-Client-LanguagePack-Package
31bf3856ad364e35
amd64
en-US
10.0.19041.746
```

That's much easier to read now.  Once you see the pattern, every Windows Package and Windows Capability follows the same structure

```
[0]ProductName
[1]PublicKeyToken
[2]Architecture
[3]Culture
[4]Version
```

## Get-MyWindowsPackage

This is a new function in the OSD Module that will split the PackageName making things easier to read.  Simply run the following command (-Online is assumed if a Path is not given)

```
Get-MyWindowsPackage | ft
```

![Get-MyWindowsPackage | ft](<../../../.gitbook/assets/image (230).png>)

Here is a comparison of **`Get-WindowsPackage`** and **`Get-MyWindowsPackage`**

```
PS C:\> Get-WindowsPackage -Online | Select -First 1

PackageName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package~31bf3856ad364e35~amd64~~10.0.19041.488
PackageState : Superseded
ReleaseType  : OnDemandPack
InstallTime  : 12/3/2020 11:51:00 PM



PS C:\> Get-MyWindowsPackage | Select -First 1

ProductName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package
Architecture : amd64
Culture      : 
Version      : 10.0.19041.488
ReleaseType  : OnDemandPack
PackageState : Superseded
InstallTime  : 12/3/2020 11:51:00 PM
PackageName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package~31bf3856ad364e35~amd64~~10.0.19041.488
Online       : True
```

## Full Details

Another cool thing you can do with **`Get-WindowsPackage`** is to give it a **`PackageName`**, and it will give you as much detail as you can handle

```
PS C:\> Get-WindowsPackage -Online -PackageName Microsoft-Windows-LanguageFeatures-Basic-en-us-Package~31bf3856ad364e35~amd64~~10.0.19041.1

PackageName              : Microsoft-Windows-LanguageFeatures-Basic-en-us-Package~31bf3856ad364e35~amd64~~10.0.19041.1
Applicable               : True
Copyright                : Copyright (c) Microsoft Corporation. All Rights Reserved
Company                  : 
CreationTime             : 
Description              : Spelling, text prediction, and document searching for English (US)
InstallClient            : DISM Package Manager Provider
InstallPackageName       : Microsoft-Windows-LanguageFeatures-Basic-en-us-Package~31bf3856ad364e35~amd64~~10.0.19041.1.mum
InstallTime              : 12/7/2019 9:50:57 AM
LastUpdateTime           : 
DisplayName              : English (US) typing
ProductName              : Microsoft-Windows-LanguageFeatures-Basic-en-us-Package
ProductVersion           : 
ReleaseType              : OnDemandPack
RestartRequired          : Possible
SupportInformation       : http://support.microsoft.com/?kbid=777777
PackageState             : Installed
CompletelyOfflineCapable : Undetermined
CapabilityId             : Language.Basic~~~en-US~0.0.1.0
Custom Properties        : 
                           mum2:OptionalFeatures\SchemaVersion : 1.0
                           mum2:OptionalFeatures\mum2:SettingsPageOptions\Visibility : installed
                           mum2:OptionalFeatures\mum2:SettingsPageOptions\FeatureType : language
                           mum2:OptionalFeatures\mum2:SettingsPageOptions\ManageFeatureSettings : page=SettingsPageTimeRegionLanguage
                           
Features                 : {}
```

The same method works with **`Get-WindowsCapability`** as well

```
PS C:\> Get-WindowsCapability -Online -Name Rsat.StorageMigrationService.Management.Tools~~~~0.0.1.0


Name         : Rsat.StorageMigrationService.Management.Tools~~~~0.0.1.0
State        : Installed
DisplayName  : RSAT: Storage Migration Service Management Tools
Description  : Provides management tools for storage migration jobs.
DownloadSize : 124644
InstallSize  : 834498
```

### -Detail

To make things easier, **`Get-MyWindowsPackage`** and **`Get-MyWindowsCapability`** will do a foreach and get much of these details (this can take some time) by adding the **`-Detail`** parameter

```
PS C:\> Get-WindowsPackage -Online | Select -First 1

PackageName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package~31bf3856ad364e35~amd64~~10.0.19041.488
PackageState : Superseded
ReleaseType  : OnDemandPack
InstallTime  : 12/3/2020 11:51:00 PM



PS C:\> Get-MyWindowsPackage | Select -First 1

ProductName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package
Architecture : amd64
Culture      : 
Version      : 10.0.19041.488
ReleaseType  : OnDemandPack
PackageState : Superseded
InstallTime  : 12/3/2020 11:51:00 PM
PackageName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package~31bf3856ad364e35~amd64~~10.0.19041.488
Online       : True



PS C:\> Get-MyWindowsPackage -Detail | Select -First 1

DisplayName  : Exchange ActiveSync and Internet Mail Sync engine
Architecture : amd64
Culture      : 
Version      : 10.0.19041.488
ReleaseType  : OnDemandPack
PackageState : Superseded
InstallTime  : 12/3/2020 11:51:00 PM
CapabilityId : OneCoreUAP.OneSync~~~~0.0.1.0
Description  : OS sync engine for syncing mail, contacts and calendar data. This sync engine is used by UWP apps like Mail, Calendar and People
PackageName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package~31bf3856ad364e35~amd64~~10.0.19041.488
Online       : True
ProductName  : Microsoft-OneCore-ApplicationModel-Sync-Desktop-FOD-Package
```

## Wrapping Up

Take the time to play around with the new functions and see what you can do with it

```
PS C:\> (Get-MyWindowsCapability -Category Rsat -Detail).DisplayName
RSAT: Active Directory Domain Services and Lightweight Directory Services Tools
RSAT: BitLocker Drive Encryption Administration Utilities
RSAT: Active Directory Certificate Services Tools
RSAT: DHCP Server Tools
RSAT: DNS Server Tools
RSAT: Failover Clustering Tools
RSAT: File Services Tools
RSAT: Group Policy Management Tools
RSAT: IP Address Management (IPAM) Client
RSAT: Data Center Bridging LLDP Tools
RSAT: Network Controller Management Tools
RSAT: Network Load Balancing Tools
RSAT: Remote Access Management Tools
RSAT: Remote Desktop Services Tools
RSAT: Server Manager
RSAT: Shielded VM Tools
RSAT: Storage Migration Service Management Tools
RSAT: Storage Replica Module for Windows PowerShell
RSAT: System Insights Module for Windows PowerShell
RSAT: Volume Activation Tools
RSAT: Windows Server Update Services Tools
```

![](<../../../.gitbook/assets/image (328).png>)

{% embed url="https://osd.osdeploy.com/module/functions/dism/get-mywindowscapability" %}

{% embed url="https://osd.osdeploy.com/module/functions/dism/get-mywindowspackage" %}

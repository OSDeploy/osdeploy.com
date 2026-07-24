# OSD Module

The OSD module is a maintained PowerShell module that provides WinPE-based Windows deployment and is the original home of OSDCloud (v1).

{% hint style="warning" %}
For OSDCloud deployments, use the **OSDCloud** module instead of OSD. The OSDCloud module supersedes the OSDCloud v1 implementation in OSD. The OSD module is maintained but the OSDCloud module is preferred for all new work.
{% endhint %}

| Property | Value |
|---|---|
| Publisher | Community / David Segura |
| Gallery | [powershellgallery.com/packages/OSD](https://www.powershellgallery.com/packages/OSD/) |
| Platform | WinPE |
| Architecture | amd64 / arm64 |
| Status | Maintained (OSDCloud v1 / legacy) |
| Functions Documented | 346 |

{% embed url="https://www.powershellgallery.com/packages/OSD/" %}

## Overview

The OSD module was the original host of OSDCloud. The OSDCloud functionality was later extracted into the dedicated **OSDCloud** module, which is the current, recommended choice for all new deployments. The OSD module continues to be maintained and is required for environments that have existing OSDCloud v1 workflows or scripts built against it. For any new deployment project, start with the OSDCloud module.

## When to Use the OSD Module

* Your existing scripts use `Start-OSDCloud` or other OSD-based OSDCloud v1 commands
* Your WinPE image was built with OSD-based tooling and you have not yet migrated
* You have a specific OSD function dependency outside of OSDCloud v1

## Install

```powershell
Install-Module -Name OSD -Force -SkipPublisherCheck
```

***

## Related

* [OSD on PowerShell Gallery](https://www.powershellgallery.com/packages/OSD/)
* [OSDCloud Module](../osdcloud/README.md) — Current, recommended module for OSDCloud deployments
* [OSDeploy Module](../osdeploy/README.md) — Boot image creation on Windows 11

***

## Functions

The OSD module exports a large public function set. Use the groups below to find each function reference page.

### Add Functions

| Function | Description |
|---|---|
| [Add-7Zip2BootImage](Add-7Zip2BootImage.md) | Adds 7-Zip command-line binaries to a mounted Windows image. |
| [Add-WindowsPackageSSU](Add-WindowsPackageSSU.md) | Adds a Servicing Stack Update package to Windows. |

### Backup Functions

| Function | Description |
|---|---|
| [Backup-DiskToFFU](Backup-DiskToFFU.md) | Captures a physical disk to a Full Flash Update (FFU) image. |
| [Backup-MyBitLockerKeys](Backup-MyBitLockerKeys.md) | Saves available BitLocker key materials to one or more folders. |

### Block Functions

| Function | Description |
|---|---|
| [Block-AdminUser](Block-AdminUser.md) | Blocks execution if the current user has Administrator rights |
| [Block-ManufacturerNeLenovo](Block-ManufacturerNeLenovo.md) | Blocks execution if the computer is not manufactured by Lenovo |
| [Block-NoCurl](Block-NoCurl.md) | Blocks execution if curl.exe is not available |
| [Block-NoInternet](Block-NoInternet.md) | Blocks execution if internet connectivity is not available |
| [Block-PowerShellVersionLt5](Block-PowerShellVersionLt5.md) | Blocks execution if PowerShell version is less than 5 |
| [Block-StandardUser](Block-StandardUser.md) | Blocks execution if the current user does not have Administrator rights |
| [Block-WindowsReleaseIdLt1703](Block-WindowsReleaseIdLt1703.md) | Blocks execution if Windows ReleaseId is less than 1703 |
| [Block-WindowsVersionNe10](Block-WindowsVersionNe10.md) | Blocks execution if Windows major version is not 10 |
| [Block-WinOS](Block-WinOS.md) | Blocks execution if the system is not running WinPE |
| [Block-WinPE](Block-WinPE.md) | Blocks execution if the system is running WinPE |

### Clear Functions

| Function | Description |
|---|---|
| [Clear-LocalDisk](Clear-LocalDisk.md) | Clears LocalDisk data or state. |
| [Clear-USBDisk](Clear-USBDisk.md) | Clears USBDisk data or state. |

### Connect Functions

| Function | Description |
|---|---|
| [Connect-OSDCloudAzure](Connect-OSDCloudAzure.md) | Connect to Azure and initialize OSDCloudAzure session state. |

### Convert Functions

| Function | Description |
|---|---|
| [Convert-EsdToFolder](Convert-EsdToFolder.md) | Expands an ESD file into a Windows setup folder structure. |
| [Convert-EsdToIso](Convert-EsdToIso.md) | Converts an ESD file into an ISO image. |
| [Convert-EsdToWim](Convert-EsdToWim.md) | Converts an ESD file into a WIM image. |
| [Convert-FolderToIso](Convert-FolderToIso.md) | Creates an ISO file from a source folder. |
| [Convert-PNPDeviceIDtoGuid](Convert-PNPDeviceIDtoGuid.md) | Extracts GUID values from a PNP Device ID string. |

### ConvertTo Functions

| Function | Description |
|---|---|
| [ConvertTo-PSKeyVaultSecret](ConvertTo-PSKeyVaultSecret.md) | Converts a value to an Azure Key Vault Secret |

### Copy Functions

| Function | Description |
|---|---|
| [Copy-IsoToUsb](Copy-IsoToUsb.md) | Creates a bootable USB drive from a Windows ISO. |
| [Copy-PSModuleToFolder](Copy-PSModuleToFolder.md) | Copies PowerShell modules to a destination module path. |
| [Copy-PSModuleToWim](Copy-PSModuleToWim.md) | Copies PowerShell modules into an offline Windows image. |
| [Copy-PSModuleToWindowsImage](Copy-PSModuleToWindowsImage.md) | Copies PowerShell modules to a mounted Windows image |
| [Copy-WinREWIM](Copy-WinREWIM.md) | Copies the Windows Recovery Environment WIM to the specified DestinationDirectory |

### Dismount Functions

| Function | Description |
|---|---|
| [Dismount-MyWindowsImage](Dismount-MyWindowsImage.md) | Dismounts MyWindowsImage and finalizes changes. |

### Edit Functions

| Function | Description |
|---|---|
| [Edit-AdkWinPEWIM](Edit-AdkWinPEWIM.md) | Adds PowerShell and PowerShell Gallery support to ADK's x64 winpe.wim |
| [Edit-MyWindowsImage](Edit-MyWindowsImage.md) | Edits MyWindowsImage content. |
| [Edit-MyWinPE](Edit-MyWinPE.md) | Mounts and edits a WinPE WIM file |
| [Edit-OSDCloudWinPE](Edit-OSDCloudWinPE.md) | Edits content by using Edit-OSDCloudWinPE. |

### Enable Functions

| Function | Description |
|---|---|
| [Enable-OSDCloudODT](Enable-OSDCloudODT.md) | Enables ODT Support in an OSDCloud Workspace |
| [Enable-PEWimPSGallery](Enable-PEWimPSGallery.md) | Enables PowerShell Gallery functionality in a WinPE WIM file |
| [Enable-PEWindowsImagePSGallery](Enable-PEWindowsImagePSGallery.md) | Enables PowerShell Gallery in a mounted Windows image |
| [Enable-SpecializeDriverPack](Enable-SpecializeDriverPack.md) | Configures driver pack expansion during Windows Specialize phase |

### Expand Functions

| Function | Description |
|---|---|
| [Expand-StagedDriverPack](Expand-StagedDriverPack.md) | Expands staged driver pack archives during Windows Setup |
| [Expand-ZTIDriverPack](Expand-ZTIDriverPack.md) | Expands driver packs during Lite Touch or Zero Touch deployment |

### Export Functions

| Function | Description |
|---|---|
| [Export-OSDCertificatesAsReg](Export-OSDCertificatesAsReg.md) | Exports selected LocalMachine certificates as .reg files. |

### Find Functions

| Function | Description |
|---|---|
| [Find-OSDCloudFile](Find-OSDCloudFile.md) | No synopsis provided. |
| [Find-OSDCloudODTFile](Find-OSDCloudODTFile.md) | No synopsis provided. |
| [Find-OSDCloudOfflineFile](Find-OSDCloudOfflineFile.md) | No synopsis provided. |
| [Find-OSDCloudOfflinePath](Find-OSDCloudOfflinePath.md) | No synopsis provided. |
| [Find-TextInFile](Find-TextInFile.md) | Searches files for matching text and displays selectable results. |
| [Find-TextInModule](Find-TextInModule.md) | Searches module files for matching text. |

### Get Functions

| Function | Description |
|---|---|
| [Get-AzClipboard](Get-AzClipboard.md) | Read a secret value from the Azure clipboard Key Vault. |
| [Get-AzOSDCloud](Get-AzOSDCloud.md) | Initialize the local OSDCloud Azure workspace. |
| [Get-AzOSDTechId](Get-AzOSDTechId.md) | Find Azure AD users for an OSD tech identifier prefix. |
| [Get-CimVideoControllerResolution](Get-CimVideoControllerResolution.md) | Returns CIM video controller resolution entries for the system display adapter. |
| [Get-CloudSecret](Get-CloudSecret.md) | Read a secret from Azure Key Vault. |
| [Get-ComObjects](Get-ComObjects.md) | Lists registered COM ProgIDs from the local machine registry. |
| [Get-ComObjMicrosoftUpdateAutoUpdate](Get-ComObjMicrosoftUpdateAutoUpdate.md) | Gets Microsoft Update automatic update settings through COM. |
| [Get-ComObjMicrosoftUpdateInstaller](Get-ComObjMicrosoftUpdateInstaller.md) | Creates and returns the Microsoft Update installer COM object. |
| [Get-ComObjMicrosoftUpdateServiceManager](Get-ComObjMicrosoftUpdateServiceManager.md) | Gets Windows Update service registration details through COM. |
| [Get-DataDisk](Get-DataDisk.md) | Gets DataDisk information. |
| [Get-DellApplicationCatalog](Get-DellApplicationCatalog.md) | Returns the Application Component of the Dell System Catalog |
| [Get-DellBiosCatalog](Get-DellBiosCatalog.md) | Returns the BIOS Component of the Dell System Catalog |
| [Get-DellDriverCatalog](Get-DellDriverCatalog.md) | Returns the Driver Component of the Dell System Catalog |
| [Get-DellDriverPackCatalog](Get-DellDriverPackCatalog.md) | Returns the Dell DriverPack Catalog |
| [Get-DellFirmwareCatalog](Get-DellFirmwareCatalog.md) | Returns the Firmware Component of the Dell System Catalog |
| [Get-DellSystemCatalog](Get-DellSystemCatalog.md) | Builds the Dell System Catalog |
| [Get-DellWinPE10DriverPack](Get-DellWinPE10DriverPack.md) | Returns the URL of the latest Dell WinPE 10 Driver Pack |
| [Get-DellWinPEDriverPack](Get-DellWinPEDriverPack.md) | Returns the URL of the latest Dell WinPE 11 Driver Pack |
| [Get-DisplayAllScreens](Get-DisplayAllScreens.md) | Returns all display screens on the system |
| [Get-DisplayPrimaryBitmapSize](Get-DisplayPrimaryBitmapSize.md) | Returns the primary display bitmap size accounting for DPI scaling |
| [Get-DisplayPrimaryMonitorSize](Get-DisplayPrimaryMonitorSize.md) | Returns the primary display monitor size in pixels |
| [Get-DisplayPrimaryScaling](Get-DisplayPrimaryScaling.md) | Returns the DPI scaling percentage of the primary display |
| [Get-DisplayVirtualScreen](Get-DisplayVirtualScreen.md) | Returns the virtual screen dimensions covering all displays |
| [Get-DownLinks](Get-DownLinks.md) | Gets a list of links to download |
| [Get-EnablementPackage](Get-EnablementPackage.md) | Returns the latest matching Windows enablement package metadata. |
| [Get-FeatureUpdate](Get-FeatureUpdate.md) | Returns the latest matching Windows client feature update record. |
| [Get-GithubRawContent](Get-GithubRawContent.md) | Retrieves content from GitHub or Gist raw URLs. |
| [Get-GithubRawUrl](Get-GithubRawUrl.md) | Resolves a GitHub or Gist URL to one or more raw content URLs. |
| [Get-HPAccessoryCatalog](Get-HPAccessoryCatalog.md) | Returns the 'Accessories Firmware and Driver' Component of the HP System Catalog |
| [Get-HPBiosCatalog](Get-HPBiosCatalog.md) | Returns the BIOS Component of the HP System Catalog |
| [Get-HPDeviceFamilyPlatformDetails](Get-HPDeviceFamilyPlatformDetails.md) | No synopsis provided. |
| [Get-HPDriverCatalog](Get-HPDriverCatalog.md) | Returns the Driver Component of the HP System Catalog |
| [Get-HPDriverPackCatalog](Get-HPDriverPackCatalog.md) | Returns the HP DriverPack Catalog |
| [Get-HPDriverPackLatest](Get-HPDriverPackLatest.md) | Gets the latest available HP driver pack for a platform. |
| [Get-HPFirmwareCatalog](Get-HPFirmwareCatalog.md) | Returns the Firmware Component of the HP System Catalog |
| [Get-HPIAJSONResult](Get-HPIAJSONResult.md) | No synopsis provided. |
| [Get-HPIALatestVersion](Get-HPIALatestVersion.md) | No synopsis provided. |
| [Get-HPIAXMLResult](Get-HPIAXMLResult.md) | No synopsis provided. |
| [Get-HPOSSupport](Get-HPOSSupport.md) | Gets supported Windows releases for an HP platform from the HPIA catalog. |
| [Get-HPPlatformCatalog](Get-HPPlatformCatalog.md) | Converts the HP Platform list to a PowerShell Object. |
| [Get-HPSoftPaqItems](Get-HPSoftPaqItems.md) | Gets HPIA SoftPaq items for a specific HP platform and OS release. |
| [Get-HPSoftpaqListLatest](Get-HPSoftpaqListLatest.md) | Gets the latest HPIA SoftPaq list for an HP platform. |
| [Get-HPSoftwareCatalog](Get-HPSoftwareCatalog.md) | Returns the Software Component of the HP System Catalog |
| [Get-HPSystemCatalog](Get-HPSystemCatalog.md) | Converts the HP Client Catalog for Microsoft System Center Product to a PowerShell Object |
| [Get-HPTPMDetermine](Get-HPTPMDetermine.md) | Determines which HP TPM firmware update package is required for the current device. |
| [Get-HpWinPEDriverPack](Get-HpWinPEDriverPack.md) | Returns the URL of the latest HP WinPE 10 Driver Pack |
| [Get-HyperVName](Get-HyperVName.md) | No synopsis provided. |
| [Get-IntelEthernetDriverPack](Get-IntelEthernetDriverPack.md) | Returns the Intel Ethernet Driver Object |
| [Get-IntelGraphicsDriverPack](Get-IntelGraphicsDriverPack.md) | Returns the Intel Graphics Driver Object |
| [Get-IntelRadeonDriverPack](Get-IntelRadeonDriverPack.md) | Returns the Intel Radeon Graphics Driver Object |
| [Get-IntelWirelessDriverPack](Get-IntelWirelessDriverPack.md) | Returns the Intel Wireless Driver Object |
| [Get-LenovoBiosCatalog](Get-LenovoBiosCatalog.md) | Builds the Lenovo Bios Catalog |
| [Get-LenovoDriverPackCatalog](Get-LenovoDriverPackCatalog.md) | Returns the Lenovo DriverPack Catalog |
| [Get-LocalDisk](Get-LocalDisk.md) | Gets LocalDisk information. |
| [Get-LocalDiskPartition](Get-LocalDiskPartition.md) | Gets LocalDiskPartition information. |
| [Get-LocalDiskVolume](Get-LocalDiskVolume.md) | Gets LocalDiskVolume information. |
| [Get-MsUpCat](Get-MsUpCat.md) | Retrieves Microsoft updates from the Microsoft Update Catalog |
| [Get-MsUpCatUpdate](Get-MsUpCatUpdate.md) | Retrieves updates for a specific Windows operating system version from Microsoft Update Catalog |
| [Get-MyBiosSerialNumber](Get-MyBiosSerialNumber.md) | Gets MyBiosSerialNumber information. |
| [Get-MyBiosUpdate](Get-MyBiosUpdate.md) | Gets MyBiosUpdate information. |
| [Get-MyBiosVersion](Get-MyBiosVersion.md) | Gets MyBiosVersion information. |
| [Get-MyBitLockerKeyProtectors](Get-MyBitLockerKeyProtectors.md) | Returns BitLocker key protector details for encrypted volumes. |
| [Get-MyComputerManufacturer](Get-MyComputerManufacturer.md) | Gets MyComputerManufacturer information. |
| [Get-MyComputerModel](Get-MyComputerModel.md) | Gets MyComputerModel information. |
| [Get-MyComputerProduct](Get-MyComputerProduct.md) | Gets MyComputerProduct information. |
| [Get-MyDefaultAUService](Get-MyDefaultAUService.md) | Returns the Default AU Service from Microsoft.Update.ServiceManager |
| [Get-MyDellBios](Get-MyDellBios.md) | Returns the latest compatible Dell BIOS update for the current system. |
| [Get-MyDriverPack](Get-MyDriverPack.md) | Retrieves the driver pack for the current computer from OSDCloud |
| [Get-MyWindowsCapability](Get-MyWindowsCapability.md) | Gets MyWindowsCapability information. |
| [Get-MyWindowsPackage](Get-MyWindowsPackage.md) | Gets MyWindowsPackage information. |
| [Get-NativeMatchineImage](Get-NativeMatchineImage.md) | Gets NativeMatchineImage information. |
| [Get-OSD](Get-OSD.md) | Displays information about the OSD Module |
| [Get-OSDClass](Get-OSDClass.md) | Returns CimInstance information from common OSD Classes |
| [Get-OSDCloudAzureResources](Get-OSDCloudAzureResources.md) | Discover OSDCloud Azure Storage resources. |
| [Get-OSDCloudDriverPack](Get-OSDCloudDriverPack.md) | Gets the OSDCloud DriverPack for the current or specified computer model |
| [Get-OSDCloudDriverPacks](Get-OSDCloudDriverPacks.md) | Returns the DriverPacks used by OSDCloud |
| [Get-OSDCloudOperatingSystems](Get-OSDCloudOperatingSystems.md) | Gets OSDCloud operating system entries for a specific architecture. |
| [Get-OSDCloudOperatingSystemsIndexes](Get-OSDCloudOperatingSystemsIndexes.md) | Returns OSDCloud operating system index entries by architecture. |
| [Get-OSDCloudOperatingSystemsIndexMap](Get-OSDCloudOperatingSystemsIndexMap.md) | Returns OSDCloud operating system index map entries by architecture. |
| [Get-OSDCloudOSNames](Get-OSDCloudOSNames.md) | Returns the Operating Systems names used by OSDCloud |
| [Get-OSDCloudTemplate](Get-OSDCloudTemplate.md) | Gets information returned by Get-OSDCloudTemplate. |
| [Get-OSDCloudTemplateNames](Get-OSDCloudTemplateNames.md) | Gets information returned by Get-OSDCloudTemplateNames. |
| [Get-OSDCloudVMDefaults](Get-OSDCloudVMDefaults.md) | Gets the OSDCloudVM Module defaults from $Global:OSDModuleResource.NewOSDCloudVM |
| [Get-OSDCloudVMSettings](Get-OSDCloudVMSettings.md) | Gets information returned by Get-OSDCloudVMSettings. |
| [Get-OSDCloudWorkspace](Get-OSDCloudWorkspace.md) | Gets information returned by Get-OSDCloudWorkspace. |
| [Get-OSDCoreCacheContent](Get-OSDCoreCacheContent.md) | Returns cached OSDCloud content found on local file system drives. |
| [Get-OSDCoreCacheDrive](Get-OSDCoreCacheDrive.md) | Returns OSDCloud cache drive metadata from local file system drives. |
| [Get-OSDCoreCacheUSBPath](Get-OSDCoreCacheUSBPath.md) | Returns OSDCloud cache paths located on USB drives. |
| [Get-OSDCoreDeploymentDisk](Get-OSDCoreDeploymentDisk.md) | Retrieves disk objects suitable for OS deployment with enhanced filtering capabilities. |
| [Get-OSDCoreDriverPackCatalogDell](Get-OSDCoreDriverPackCatalogDell.md) | Downloads and parses the Dell driver pack catalog for Windows 11. |
| [Get-OSDCoreDriverPackCatalogHP](Get-OSDCoreDriverPackCatalogHP.md) | Downloads and parses the HP driver pack catalog for Windows 11. |
| [Get-OSDCoreDriverPackCatalogLenovo](Get-OSDCoreDriverPackCatalogLenovo.md) | Downloads and parses the Lenovo driver pack catalog for Windows 11. |
| [Get-OSDCoreDriverPackCatalogPanasonic](Get-OSDCoreDriverPackCatalogPanasonic.md) | No synopsis provided. |
| [Get-OSDCoreDriverPackCatalogSurface](Get-OSDCoreDriverPackCatalogSurface.md) | Retrieves the Microsoft Surface driver pack catalog, enriching entries from live download pages. |
| [Get-OSDCoreDriverPacks](Get-OSDCoreDriverPacks.md) | Retrieves driver pack information for the specified manufacturer and operating system architecture. |
| [Get-OSDCoreLicense](Get-OSDCoreLicense.md) | Returns a single Recast Core license object. |
| [Get-OSDCoreOperatingSystems](Get-OSDCoreOperatingSystems.md) | Gets the core operating system catalog entries that OSD uses for offline media selection. |
| [Get-OSDDisk](Get-OSDDisk.md) | Gets OSDDisk information. |
| [Get-OSDGather](Get-OSDGather.md) | Returns common OSD information as an ordered hash table |
| [Get-OSDHelp](Get-OSDHelp.md) | Gets OSDHelp information. |
| [Get-OSDMetrics](Get-OSDMetrics.md) | Retrieves metrics for the OSD PowerShell module and OSDCloud deployment methods. |
| [Get-OSDModuleCache](Get-OSDModuleCache.md) | Returns the OSD module cache directory path. |
| [Get-OSDModulePath](Get-OSDModulePath.md) | Returns the base path of the loaded OSD module. |
| [Get-OSDModuleVersion](Get-OSDModuleVersion.md) | Returns the version of the loaded OSD module. |
| [Get-OSDPad](Get-OSDPad.md) | Gets information returned by Get-OSDPad. |
| [Get-OSDPartition](Get-OSDPartition.md) | Gets OSDPartition information. |
| [Get-OSDPower](Get-OSDPower.md) | Displays Power Plan information using powercfg /LIST |
| [Get-OSDVolume](Get-OSDVolume.md) | Gets OSDVolume information. |
| [Get-OSDWinEvent](Get-OSDWinEvent.md) | Gets OSDWinEvent information. |
| [Get-PowerSettingSleepAfter](Get-PowerSettingSleepAfter.md) | No synopsis provided. |
| [Get-PowerSettingTurnMonitorOffAfter](Get-PowerSettingTurnMonitorOffAfter.md) | Gets the active power plan monitor-off timeout in minutes. |
| [Get-PSCloudScript](Get-PSCloudScript.md) | Development function to get the contents of a PSCloudScript. |
| [Get-ReAgentXml](Get-ReAgentXml.md) | Returns information from the Windows Recovery Agent XML file |
| [Get-RegCurrentVersion](Get-RegCurrentVersion.md) | Returns the Registry Key values from HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion |
| [Get-ScreenPNG](Get-ScreenPNG.md) | Gets ScreenPNG information. |
| [Get-SessionsXml](Get-SessionsXml.md) | Returns the Session.xml Updates that have been applied to an Operating System |
| [Get-SetupCompleteOSDCloudUSB](Get-SetupCompleteOSDCloudUSB.md) | This function checks for the presence of an OSDCloud SetupComplete Folder on any drive other than 'C'. |
| [Get-SurfaceDriverPackCatalog](Get-SurfaceDriverPackCatalog.md) | Returns the Microsoft Surface DriverPack Catalog |
| [Get-SystemFirmwareDevice](Get-SystemFirmwareDevice.md) | Returns the system firmware device |
| [Get-SystemFirmwareResource](Get-SystemFirmwareResource.md) | Returns the GUID of the system firmware resource |
| [Get-SystemFirmwareUpdate](Get-SystemFirmwareUpdate.md) | Retrieves the latest system firmware update from Microsoft Update Catalog |
| [Get-TimeZoneFromIP](Get-TimeZoneFromIP.md) | No synopsis provided. |
| [Get-USBDisk](Get-USBDisk.md) | Gets USBDisk information. |
| [Get-USBPartition](Get-USBPartition.md) | Gets USBPartition information. |
| [Get-USBVolume](Get-USBVolume.md) | Gets USBVolume information. |
| [Get-WiFiActiveProfileSSID](Get-WiFiActiveProfileSSID.md) | No synopsis provided. |
| [Get-WiFiProfileKey](Get-WiFiProfileKey.md) | No synopsis provided. |
| [Get-Win11Readiness](Get-Win11Readiness.md) | No synopsis provided. |
| [Get-WindowsAdkInstallPath](Get-WindowsAdkInstallPath.md) | Retrieves the installation path of the Windows Assessment and Deployment Kit (ADK) |
| [Get-WindowsAdkInstallVersion](Get-WindowsAdkInstallVersion.md) | Retrieves the installed version of the Windows Assessment and Deployment Kit (ADK) |
| [Get-WindowsAdkPaths](Get-WindowsAdkPaths.md) | Retrieves the command paths of the Windows Assessment and Deployment Kit (ADK). |
| [Get-WindowsKitsInstallPath](Get-WindowsKitsInstallPath.md) | Retrieves the installation path of the Windows Kit directory. |
| [Get-WindowsOEMProductKey](Get-WindowsOEMProductKey.md) | No synopsis provided. |
| [Get-WindowsUpdateDriver](Get-WindowsUpdateDriver.md) | No synopsis provided. |
| [Get-WindowsUpdateManifests](Get-WindowsUpdateManifests.md) | Returns an Array of Microsoft Updates from the Microsoft Update Catalog |
| [Get-WinREPartition](Get-WinREPartition.md) | Retrieves the Windows Recovery Environment partition information |
| [Get-WSUSXML](Get-WSUSXML.md) | Returns an Array of Microsoft Updates |

### Import Functions

| Function | Description |
|---|---|
| [Import-MDTWinPECloudDriver](Import-MDTWinPECloudDriver.md) | Imports OSDCloud CloudDrivers into an MDT Deployment Share |
| [Import-OSDCloudWinPEDriverMDT](Import-OSDCloudWinPEDriverMDT.md) | Imports OSDCloud CloudDrivers into an MDT Deployment Share |

### Initialize Functions

| Function | Description |
|---|---|
| [Initialize-OSDCloudStartnet](Initialize-OSDCloudStartnet.md) | Initializes the OSDCloud startnet environment. |
| [Initialize-OSDCloudStartnetUpdate](Initialize-OSDCloudStartnetUpdate.md) | No synopsis provided. |
| [Initialize-OSDCoreDevice](Initialize-OSDCoreDevice.md) | Collects local hardware, firmware, TPM, and network details for OSDCloud. |

### Install Functions

| Function | Description |
|---|---|
| [Install-AzOSDIacTools](Install-AzOSDIacTools.md) | Install prerequisite IaC tooling for OSDCloud Azure. |
| [Install-BuildUpdatesFromOSCloudUSB](Install-BuildUpdatesFromOSCloudUSB.md) | No synopsis provided. |
| [Install-HPIA](Install-HPIA.md) | No synopsis provided. |
| [Install-ModuleHPCMSL](Install-ModuleHPCMSL.md) | Installs or updates the HP Client Management Script Library (HPCMSL) PowerShell module. |
| [Install-SystemFirmwareUpdate](Install-SystemFirmwareUpdate.md) | Downloads and installs the system firmware update |

### Invoke Functions

| Function | Description |
|---|---|
| [Invoke-AzOSDAzureConfig](Invoke-AzOSDAzureConfig.md) | Deploy OSDCloud Azure infrastructure with Bicep or Terraform. |
| [Invoke-CloudSecret](Invoke-CloudSecret.md) | Invoke a secret retrieved from Azure Key Vault. |
| [Invoke-Exe](Invoke-Exe.md) | Runs an external command. |
| [Invoke-HPAnalyzer](Invoke-HPAnalyzer.md) | No synopsis provided. |
| [Invoke-HPDriverUpdate](Invoke-HPDriverUpdate.md) | No synopsis provided. |
| [Invoke-HPIA](Invoke-HPIA.md) | No synopsis provided. |
| [Invoke-HPIAOfflineSync](Invoke-HPIAOfflineSync.md) | Creates and synchronizes an offline HPIA repository for the local HP platform. |
| [Invoke-HPTPMDowngrade](Invoke-HPTPMDowngrade.md) | Downloads and applies the HP SP94937 softpaq to downgrade a TPM from 2.0 to 1.2. |
| [Invoke-HPTPMDownload](Invoke-HPTPMDownload.md) | Downloads and extracts the required HP TPM firmware update softpaq using HPCMSL. |
| [Invoke-HPTPMEXEDownload](Invoke-HPTPMEXEDownload.md) | Downloads the required HP TPM firmware EXE to C:\OSDCloud\HP\TPM. |
| [Invoke-HPTPMEXEInstall](Invoke-HPTPMEXEInstall.md) | Extracts and installs the HP TPM firmware update from C:\OSDCloud\HP\TPM. |
| [Invoke-MSCatalogParseDate](Invoke-MSCatalogParseDate.md) | Parses a date string from Microsoft Update Catalog format |
| [Invoke-OSDCloud](Invoke-OSDCloud.md) | This is the master OSDCloud Task Sequence |
| [Invoke-OSDCloudDriverPackCM](Invoke-OSDCloudDriverPackCM.md) | Downloads a matching DriverPack to %OSDisk%\Drivers |
| [Invoke-OSDCloudDriverPackMDT](Invoke-OSDCloudDriverPackMDT.md) | Downloads a matching DriverPack to %OSDisk%\Drivers |
| [Invoke-OSDCloudDriverPackPPKG](Invoke-OSDCloudDriverPackPPKG.md) | Uses DISM in WinPE to expand and apply Driver Packs |
| [Invoke-OSDCloudIPU](Invoke-OSDCloudIPU.md) | Starts an OSDCloud in-place upgrade workflow. |
| [Invoke-OSDInfo](Invoke-OSDInfo.md) | Displays OSD information, useful in an OS Deployment |
| [Invoke-OSDSpecialize](Invoke-OSDSpecialize.md) | No synopsis provided. |
| [Invoke-OSDSpecializeDev](Invoke-OSDSpecializeDev.md) | No synopsis provided. |
| [Invoke-SelectDataDisk](Invoke-SelectDataDisk.md) | Invokes SelectDataDisk actions. |
| [Invoke-SelectFFUDisk](Invoke-SelectFFUDisk.md) | Invokes SelectFFUDisk actions. |
| [Invoke-SelectLocalDisk](Invoke-SelectLocalDisk.md) | Invokes SelectLocalDisk actions. |
| [Invoke-SelectLocalVolume](Invoke-SelectLocalVolume.md) | Invokes SelectLocalVolume actions. |
| [Invoke-SelectOSDDisk](Invoke-SelectOSDDisk.md) | Invokes SelectOSDDisk actions. |
| [Invoke-SelectOSDVolume](Invoke-SelectOSDVolume.md) | Invokes SelectOSDVolume actions. |
| [Invoke-SelectUSBDisk](Invoke-SelectUSBDisk.md) | Invokes SelectUSBDisk actions. |
| [Invoke-SelectUSBVolume](Invoke-SelectUSBVolume.md) | Invokes SelectUSBVolume actions. |
| [Invoke-WebPSScript](Invoke-WebPSScript.md) | Executes a PowerShell script from a URL. |

### Mount Functions

| Function | Description |
|---|---|
| [Mount-MyWindowsImage](Mount-MyWindowsImage.md) | Mounts MyWindowsImage for servicing. |

### New Functions

| Function | Description |
|---|---|
| [New-AdkCopyPE](New-AdkCopyPE.md) | Creates an ADK CopyPE working directory |
| [New-AdkISO](New-AdkISO.md) | Creates an ISO file from a bootable media directory using ADK tools |
| [New-BootableUSBDrive](New-BootableUSBDrive.md) | Creates BootableUSBDrive resources. |
| [New-CAB](New-CAB.md) | Creates a CAB file from a Directory |
| [New-OSDCloudISO](New-OSDCloudISO.md) | Creates an OSDCloud bootable ISO from an OSDCloud workspace. |
| [New-OSDCloudOSWimFile](New-OSDCloudOSWimFile.md) | Builds Windows setup media content for an OSDCloud feature update. |
| [New-OSDCloudTemplate](New-OSDCloudTemplate.md) | Creates resources by using New-OSDCloudTemplate. |
| [New-OSDCloudUSB](New-OSDCloudUSB.md) | Creates resources by using New-OSDCloudUSB. |
| [New-OSDCloudUSBSetupCompleteTemplate](New-OSDCloudUSBSetupCompleteTemplate.md) | Creates resources by using New-OSDCloudUSBSetupCompleteTemplate. |
| [New-OSDCloudVM](New-OSDCloudVM.md) | Creates resources by using New-OSDCloudVM. |
| [New-OSDCloudWorkspace](New-OSDCloudWorkspace.md) | Creates resources by using New-OSDCloudWorkspace. |
| [New-OSDCloudWorkspaceSetupCompleteTemplate](New-OSDCloudWorkspaceSetupCompleteTemplate.md) | Creates resources by using New-OSDCloudWorkspaceSetupCompleteTemplate. |
| [New-OSDisk](New-OSDisk.md) | Creates System \| OS \| Recovery Partitions for MBR or UEFI Drives in WinPE |
| [New-WindowsAdkISO](New-WindowsAdkISO.md) | Creates an ISO file from a bootable media directory using ADK |

### Remove Functions

| Function | Description |
|---|---|
| [Remove-AppxOnline](Remove-AppxOnline.md) | Removes AppxOnline items. |

### Reset Functions

| Function | Description |
|---|---|
| [Reset-OSDCloudVMSettings](Reset-OSDCloudVMSettings.md) | Resets configuration by using Reset-OSDCloudVMSettings. |

### Resolve Functions

| Function | Description |
|---|---|
| [Resolve-MsUrl](Resolve-MsUrl.md) | Resolves a short Microsoft aka.ms or fwlink URL. |

### Save Functions

| Function | Description |
|---|---|
| [Save-ClipboardImage](Save-ClipboardImage.md) | Saves ClipboardImage content. |
| [Save-EnablementPackage](Save-EnablementPackage.md) | Downloads a matching Windows enablement package. |
| [Save-FeatureUpdate](Save-FeatureUpdate.md) | Downloads the latest matching Windows client feature update package. |
| [Save-MsUpCatDriver](Save-MsUpCatDriver.md) | Downloads driver updates from Microsoft Update Catalog |
| [Save-MsUpCatUpdate](Save-MsUpCatUpdate.md) | Downloads updates from Microsoft Update Catalog for a specific Windows version |
| [Save-MyBitLockerExternalKey](Save-MyBitLockerExternalKey.md) | Saves BitLocker external key protectors (.BEK) to destination folders. |
| [Save-MyBitLockerKeyPackage](Save-MyBitLockerKeyPackage.md) | Saves BitLocker key packages to destination folders. |
| [Save-MyBitLockerRecoveryPassword](Save-MyBitLockerRecoveryPassword.md) | Saves BitLocker recovery passwords to text files. |
| [Save-MyDellBios](Save-MyDellBios.md) | Downloads the latest compatible Dell BIOS update to a local folder. |
| [Save-MyDellBiosFlash64W](Save-MyDellBiosFlash64W.md) | Downloads and extracts the Dell Flash64W BIOS utility. |
| [Save-MyDriverPack](Save-MyDriverPack.md) | Downloads and optionally expands the driver pack for the current computer |
| [Save-SystemFirmwareUpdate](Save-SystemFirmwareUpdate.md) | Downloads and extracts the latest system firmware update. |
| [Save-WebFile](Save-WebFile.md) | Downloads a file from the internet and returns a Get-Item Object |
| [Save-WinPECloudDriver](Save-WinPECloudDriver.md) | Download and expand WinPE Drivers |
| [Save-ZTIDriverPack](Save-ZTIDriverPack.md) | Downloads the driver pack for a computer during MDT/ConfigMgr task sequence |

### Select Functions

| Function | Description |
|---|---|
| [Select-OSDCloudAutopilotJsonItem](Select-OSDCloudAutopilotJsonItem.md) | Selects Autopilot Profiles |
| [Select-OSDCloudFileWim](Select-OSDCloudFileWim.md) | Selects Office Configuration Profiles |
| [Select-OSDCloudImageIndex](Select-OSDCloudImageIndex.md) | No synopsis provided. |
| [Select-OSDCloudODTFile](Select-OSDCloudODTFile.md) | Selects Office Configuration Profiles |

### Set Functions

| Function | Description |
|---|---|
| [Set-AzClipboard](Set-AzClipboard.md) | Write the current clipboard text to the Azure clipboard Key Vault. |
| [Set-BitlockerRegValuesXTS256](Set-BitlockerRegValuesXTS256.md) | No synopsis provided. |
| [Set-BootmgrTimeout](Set-BootmgrTimeout.md) | Sets the Windows Boot Manager timeout value in BCD. |
| [Set-ClipboardScreenshot](Set-ClipboardScreenshot.md) | Captures a screenshot and copies it to the clipboard |
| [Set-CloudSecret](Set-CloudSecret.md) | Convert content to an Azure Key Vault secret. |
| [Set-DisRes](Set-DisRes.md) | Sets the primary display screen resolution. |
| [Set-HPBIOSSetting](Set-HPBIOSSetting.md) | No synopsis provided. |
| [Set-HPTPMBIOSSettings](Set-HPTPMBIOSSettings.md) | No synopsis provided. |
| [Set-HyperVName](Set-HyperVName.md) | No synopsis provided. |
| [Set-LatestUpdatesASAPEnabled](Set-LatestUpdatesASAPEnabled.md) | No synopsis provided. |
| [Set-OSDCloudTemplate](Set-OSDCloudTemplate.md) | Sets configuration values by using Set-OSDCloudTemplate. |
| [Set-OSDCloudUnattendAuditMode](Set-OSDCloudUnattendAuditMode.md) | No synopsis provided. |
| [Set-OSDCloudUnattendAuditModeAutopilot](Set-OSDCloudUnattendAuditModeAutopilot.md) | No synopsis provided. |
| [Set-OSDCloudUnattendSpecialize](Set-OSDCloudUnattendSpecialize.md) | No synopsis provided. |
| [Set-OSDCloudUnattendSpecializeDev](Set-OSDCloudUnattendSpecializeDev.md) | No synopsis provided. |
| [Set-OSDCloudVMSettings](Set-OSDCloudVMSettings.md) | Sets configuration values by using Set-OSDCloudVMSettings. |
| [Set-OSDCloudWorkspace](Set-OSDCloudWorkspace.md) | Sets configuration values by using Set-OSDCloudWorkspace. |
| [Set-OSDxCloudUnattendSpecialize](Set-OSDxCloudUnattendSpecialize.md) | No synopsis provided. |
| [Set-PowerSettingSleepAfter](Set-PowerSettingSleepAfter.md) | No synopsis provided. |
| [Set-PowerSettingTurnMonitorOffAfter](Set-PowerSettingTurnMonitorOffAfter.md) | No synopsis provided. |
| [Set-SetupCompleteBitlocker](Set-SetupCompleteBitlocker.md) | No synopsis provided. |
| [Set-SetupCompleteCreateFinish](Set-SetupCompleteCreateFinish.md) | No synopsis provided. |
| [Set-SetupCompleteCreateStart](Set-SetupCompleteCreateStart.md) | No synopsis provided. |
| [Set-SetupCompleteDefenderUpdate](Set-SetupCompleteDefenderUpdate.md) | No synopsis provided. |
| [Set-SetupCompleteHPAppend](Set-SetupCompleteHPAppend.md) | No synopsis provided. |
| [Set-SetupCompleteHyperVName](Set-SetupCompleteHyperVName.md) | No synopsis provided. |
| [Set-SetupCompleteNetFX](Set-SetupCompleteNetFX.md) | No synopsis provided. |
| [Set-SetupCompleteOEMActivation](Set-SetupCompleteOEMActivation.md) | No synopsis provided. |
| [Set-SetupCompleteOSDCloudCustom](Set-SetupCompleteOSDCloudCustom.md) | No synopsis provided. |
| [Set-SetupCompleteOSDCloudUSB](Set-SetupCompleteOSDCloudUSB.md) | This function copies SetupComplete Files to the Local OSDCloud SetupComplete Folder |
| [Set-SetupCompleteSetWiFi](Set-SetupCompleteSetWiFi.md) | No synopsis provided. |
| [Set-SetupCompleteStartWindowsUpdate](Set-SetupCompleteStartWindowsUpdate.md) | No synopsis provided. |
| [Set-SetupCompleteStartWindowsUpdateDriver](Set-SetupCompleteStartWindowsUpdateDriver.md) | No synopsis provided. |
| [Set-SetupCompleteTimeZone](Set-SetupCompleteTimeZone.md) | No synopsis provided. |
| [Set-TimeZoneFromIP](Set-TimeZoneFromIP.md) | No synopsis provided. |
| [Set-WiFi](Set-WiFi.md) | No synopsis provided. |
| [Set-WimExecutionPolicy](Set-WimExecutionPolicy.md) | Sets the PowerShell Execution Policy of a Windows Image .wim file (Mount \| Set \| Dismount -Save) |
| [Set-WindowsImageExecutionPolicy](Set-WindowsImageExecutionPolicy.md) | Sets the PowerShell Execution Policy of a mounted Windows Image |
| [Set-WindowsOEMActivation](Set-WindowsOEMActivation.md) | No synopsis provided. |

### Show Functions

| Function | Description |
|---|---|
| [Show-MsSettings](Show-MsSettings.md) | Opens the ms-setting: URI that is specified in the Setting parameter |
| [Show-OSDCoreLicenseHelp](Show-OSDCoreLicenseHelp.md) | Displays instructions for setting the Recast Core license for OSDCloud. |
| [Show-RegistryXML](Show-RegistryXML.md) | Displays registry entries from all RegistryXML files in the Source Directory |

### Start Functions

| Function | Description |
|---|---|
| [Start-DiskImageGUI](Start-DiskImageGUI.md) | Start-DiskImageGUI function. |
| [Start-DISMFromOSDCloudUSB](Start-DISMFromOSDCloudUSB.md) | No synopsis provided. |
| [Start-EjectCD](Start-EjectCD.md) | No synopsis provided. |
| [Start-OOBEDeploy](Start-OOBEDeploy.md) | No synopsis provided. |
| [Start-OSDCloud](Start-OSDCloud.md) | Prepare and start an OSDCloud deployment session (selects image, language, edition and other options). |
| [Start-OSDCloudAzure](Start-OSDCloudAzure.md) | Start an OSDCloud deployment from Azure Storage. |
| [Start-OSDCloudCLI](Start-OSDCloudCLI.md) | Starts the OSDCloud Windows 10 or 11 Build Process from the OSD Module or a GitHub Repository |
| [Start-OSDCloudGUI](Start-OSDCloudGUI.md) | OSDCloud imaging using the command line |
| [Start-OSDCloudGUIDev](Start-OSDCloudGUIDev.md) | OSDCloud imaging using the command line |
| [Start-OSDCloudREAzure](Start-OSDCloudREAzure.md) | OSDCloudRE: Creates a new OSDCloudRE Volume from Azure |
| [Start-OSDCloudToolbox](Start-OSDCloudToolbox.md) | Starts the workflow for Start-OSDCloudToolbox. |
| [Start-OSDDiskPart](Start-OSDDiskPart.md) | No synopsis provided. |
| [Start-OSDeployPad](Start-OSDeployPad.md) | Starts the workflow for Start-OSDeployPad. |
| [Start-OSDPad](Start-OSDPad.md) | Starts the workflow for Start-OSDPad. |
| [Start-OSDPadCategories](Start-OSDPadCategories.md) | Starts the workflow for Start-OSDPadCategories. |
| [Start-RecastOSDCloudCLI](Start-RecastOSDCloudCLI.md) | Starts the Recast OSDCloud command-line deployment workflow. |
| [Start-ScreenPNGProcess](Start-ScreenPNGProcess.md) | Starts a background process to capture screenshots |
| [Start-WindowsUpdate](Start-WindowsUpdate.md) | No synopsis provided. |
| [Start-WindowsUpdateDriver](Start-WindowsUpdateDriver.md) | No synopsis provided. |

### Stop Functions

| Function | Description |
|---|---|
| [Stop-ScreenPNGProcess](Stop-ScreenPNGProcess.md) | Stops the background screenshot capture process |

### Test Functions

| Function | Description |
|---|---|
| [Test-DCUSupport](Test-DCUSupport.md) | No synopsis provided. |
| [Test-DISMFromOSDCloudUSB](Test-DISMFromOSDCloudUSB.md) | No synopsis provided. |
| [Test-DynamicValidateSet](Test-DynamicValidateSet.md) | Tests DynamicValidateSet conditions. |
| [Test-HPIASupport](Test-HPIASupport.md) | Tests whether the current HP platform is supported by HPIA. |
| [Test-HPTPMFromOSDCloudUSB](Test-HPTPMFromOSDCloudUSB.md) | Tests whether HP TPM firmware packages exist on an OSDCloud USB drive. |
| [Test-IsVM](Test-IsVM.md) | Tests IsVM conditions. |
| [Test-MicrosoftUpdateCatalog](Test-MicrosoftUpdateCatalog.md) | Tests connectivity to Microsoft Update Catalog. |
| [Test-OSDCoreCacheUSB](Test-OSDCoreCacheUSB.md) | Tests whether any OSDCloud cache drive is a USB drive. |
| [Test-OSDCoreDriverPackCloudObject](Test-OSDCoreDriverPackCloudObject.md) | Tests whether an OSDCore driver pack object URL is reachable. |
| [Test-OSDCoreOperatingSystemCloudObject](Test-OSDCoreOperatingSystemCloudObject.md) | Tests whether an OSDCore operating system object URL is reachable. |
| [Test-WebConnection](Test-WebConnection.md) | Tests web connectivity to a target URI using a live TCP connection and HTTP HEAD request. |
| [Test-WindowsImage](Test-WindowsImage.md) | Tests WindowsImage conditions. |
| [Test-WindowsImageMounted](Test-WindowsImageMounted.md) | Tests WindowsImageMounted conditions. |
| [Test-WindowsImageMountPath](Test-WindowsImageMountPath.md) | Tests WindowsImageMountPath conditions. |
| [Test-WindowsPackageCAB](Test-WindowsPackageCAB.md) | Tests WindowsPackageCAB conditions. |

### Unblock Functions

| Function | Description |
|---|---|
| [Unblock-WindowsUpdate](Unblock-WindowsUpdate.md) | Opens Windows Update and checks for WSUS configuration |

### Unlock Functions

| Function | Description |
|---|---|
| [Unlock-MyBitLockerExternalKey](Unlock-MyBitLockerExternalKey.md) | Unlocks BitLocker volumes using external key files. |

### Update Functions

| Function | Description |
|---|---|
| [Update-DefenderStack](Update-DefenderStack.md) | No synopsis provided. |
| [Update-IntelDriversCatalog](Update-IntelDriversCatalog.md) | Updates the Intel Drivers Cats in the OSD Module |
| [Update-MyDellBios](Update-MyDellBios.md) | Downloads and launches a compatible BIOS update for the current Dell system. |
| [Update-MyWindowsImage](Update-MyWindowsImage.md) | Updates MyWindowsImage content. |
| [Update-OSDCloudUSB](Update-OSDCloudUSB.md) | Updates resources by using Update-OSDCloudUSB. |
| [Update-RecastOSDCloudUSBCache](Update-RecastOSDCloudUSBCache.md) | Starts the Recast OSDCloud command-line deployment workflow. |

### Wait Functions

| Function | Description |
|---|---|
| [Wait-WebConnection](Wait-WebConnection.md) | Waits for an internet connection to the specified Uri |

### Write Functions

| Function | Description |
|---|---|
| [Write-CMTraceLog](Write-CMTraceLog.md) | No synopsis provided. |


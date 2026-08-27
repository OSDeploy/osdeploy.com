# OSD Module

The OSD module is a maintained PowerShell module that provides WinPE-based Windows deployment and is the original home of OSDCloud (v1).

{% hint style="warning" %}
For OSDCloud deployments, use the **OSDCloud** module instead of OSD. The OSDCloud module supersedes the OSDCloud v1 implementation in OSD. The OSD module is maintained but the OSDCloud module is preferred for all new work.
{% endhint %}

| Property             | Value                                                                                 |
| -------------------- | ------------------------------------------------------------------------------------- |
| Publisher            | Community / David Segura                                                              |
| Gallery              | [powershellgallery.com/packages/OSD](https://www.powershellgallery.com/packages/OSD/) |
| Platform             | WinPE                                                                                 |
| Architecture         | amd64 / arm64                                                                         |
| Status               | Maintained (OSDCloud v1 / legacy)                                                     |
| Functions Documented | 346                                                                                   |

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
* [OSDCloud Module](../osdcloud/) — Current, recommended module for OSDCloud deployments
* [OSDeploy Module](../osdeploy/) — Boot image creation on Windows 11

***

## Functions

The OSD module exports a large public function set. Use the groups below to find each function reference page.

### Add Functions

| Function                                          | Description                                                  |
| ------------------------------------------------- | ------------------------------------------------------------ |
| [Add-7Zip2BootImage](add-7zip2bootimage.md)       | Adds 7-Zip command-line binaries to a mounted Windows image. |
| [Add-WindowsPackageSSU](add-windowspackagessu.md) | Adds a Servicing Stack Update package to Windows.            |

### Backup Functions

| Function                                            | Description                                                     |
| --------------------------------------------------- | --------------------------------------------------------------- |
| [Backup-DiskToFFU](backup-disktoffu.md)             | Captures a physical disk to a Full Flash Update (FFU) image.    |
| [Backup-MyBitLockerKeys](backup-mybitlockerkeys.md) | Saves available BitLocker key materials to one or more folders. |

### Block Functions

| Function                                                        | Description                                                             |
| --------------------------------------------------------------- | ----------------------------------------------------------------------- |
| [Block-AdminUser](block-adminuser.md)                           | Blocks execution if the current user has Administrator rights           |
| [Block-ManufacturerNeLenovo](block-manufacturernelenovo.md)     | Blocks execution if the computer is not manufactured by Lenovo          |
| [Block-NoCurl](block-nocurl.md)                                 | Blocks execution if curl.exe is not available                           |
| [Block-NoInternet](block-nointernet.md)                         | Blocks execution if internet connectivity is not available              |
| [Block-PowerShellVersionLt5](block-powershellversionlt5.md)     | Blocks execution if PowerShell version is less than 5                   |
| [Block-StandardUser](block-standarduser.md)                     | Blocks execution if the current user does not have Administrator rights |
| [Block-WindowsReleaseIdLt1703](block-windowsreleaseidlt1703.md) | Blocks execution if Windows ReleaseId is less than 1703                 |
| [Block-WindowsVersionNe10](block-windowsversionne10.md)         | Blocks execution if Windows major version is not 10                     |
| [Block-WinOS](block-winos.md)                                   | Blocks execution if the system is not running WinPE                     |
| [Block-WinPE](block-winpe.md)                                   | Blocks execution if the system is running WinPE                         |

### Clear Functions

| Function                              | Description                     |
| ------------------------------------- | ------------------------------- |
| [Clear-LocalDisk](clear-localdisk.md) | Clears LocalDisk data or state. |
| [Clear-USBDisk](clear-usbdisk.md)     | Clears USBDisk data or state.   |

### Connect Functions

| Function                                          | Description                                                  |
| ------------------------------------------------- | ------------------------------------------------------------ |
| [Connect-OSDCloudAzure](connect-osdcloudazure.md) | Connect to Azure and initialize OSDCloudAzure session state. |

### Convert Functions

| Function                                                  | Description                                                |
| --------------------------------------------------------- | ---------------------------------------------------------- |
| [Convert-EsdToFolder](convert-esdtofolder.md)             | Expands an ESD file into a Windows setup folder structure. |
| [Convert-EsdToIso](convert-esdtoiso.md)                   | Converts an ESD file into an ISO image.                    |
| [Convert-EsdToWim](convert-esdtowim.md)                   | Converts an ESD file into a WIM image.                     |
| [Convert-FolderToIso](convert-foldertoiso.md)             | Creates an ISO file from a source folder.                  |
| [Convert-PNPDeviceIDtoGuid](convert-pnpdeviceidtoguid.md) | Extracts GUID values from a PNP Device ID string.          |

### ConvertTo Functions

| Function                                                    | Description                                   |
| ----------------------------------------------------------- | --------------------------------------------- |
| [ConvertTo-PSKeyVaultSecret](convertto-pskeyvaultsecret.md) | Converts a value to an Azure Key Vault Secret |

### Copy Functions

| Function                                                      | Description                                                                       |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| [Copy-IsoToUsb](copy-isotousb.md)                             | Creates a bootable USB drive from a Windows ISO.                                  |
| [Copy-PSModuleToFolder](copy-psmoduletofolder.md)             | Copies PowerShell modules to a destination module path.                           |
| [Copy-PSModuleToWim](copy-psmoduletowim.md)                   | Copies PowerShell modules into an offline Windows image.                          |
| [Copy-PSModuleToWindowsImage](copy-psmoduletowindowsimage.md) | Copies PowerShell modules to a mounted Windows image                              |
| [Copy-WinREWIM](copy-winrewim.md)                             | Copies the Windows Recovery Environment WIM to the specified DestinationDirectory |

### Dismount Functions

| Function                                              | Description                                     |
| ----------------------------------------------------- | ----------------------------------------------- |
| [Dismount-MyWindowsImage](dismount-mywindowsimage.md) | Dismounts MyWindowsImage and finalizes changes. |

### Edit Functions

| Function                                      | Description                                                           |
| --------------------------------------------- | --------------------------------------------------------------------- |
| [Edit-AdkWinPEWIM](edit-adkwinpewim.md)       | Adds PowerShell and PowerShell Gallery support to ADK's x64 winpe.wim |
| [Edit-MyWindowsImage](edit-mywindowsimage.md) | Edits MyWindowsImage content.                                         |
| [Edit-MyWinPE](edit-mywinpe.md)               | Mounts and edits a WinPE WIM file                                     |
| [Edit-OSDCloudWinPE](edit-osdcloudwinpe.md)   | Edits content by using Edit-OSDCloudWinPE.                            |

### Enable Functions

| Function                                                            | Description                                                      |
| ------------------------------------------------------------------- | ---------------------------------------------------------------- |
| [Enable-OSDCloudODT](enable-osdcloudodt.md)                         | Enables ODT Support in an OSDCloud Workspace                     |
| [Enable-PEWimPSGallery](enable-pewimpsgallery.md)                   | Enables PowerShell Gallery functionality in a WinPE WIM file     |
| [Enable-PEWindowsImagePSGallery](enable-pewindowsimagepsgallery.md) | Enables PowerShell Gallery in a mounted Windows image            |
| [Enable-SpecializeDriverPack](enable-specializedriverpack.md)       | Configures driver pack expansion during Windows Specialize phase |

### Expand Functions

| Function                                              | Description                                                     |
| ----------------------------------------------------- | --------------------------------------------------------------- |
| [Expand-StagedDriverPack](expand-stageddriverpack.md) | Expands staged driver pack archives during Windows Setup        |
| [Expand-ZTIDriverPack](expand-ztidriverpack.md)       | Expands driver packs during Lite Touch or Zero Touch deployment |

### Export Functions

| Function                                                      | Description                                               |
| ------------------------------------------------------------- | --------------------------------------------------------- |
| [Export-OSDCertificatesAsReg](export-osdcertificatesasreg.md) | Exports selected LocalMachine certificates as .reg files. |

### Find Functions

| Function                                                | Description                                                       |
| ------------------------------------------------------- | ----------------------------------------------------------------- |
| [Find-OSDCloudFile](find-osdcloudfile.md)               | No synopsis provided.                                             |
| [Find-OSDCloudODTFile](find-osdcloudodtfile.md)         | No synopsis provided.                                             |
| [Find-OSDCloudOfflineFile](find-osdcloudofflinefile.md) | No synopsis provided.                                             |
| [Find-OSDCloudOfflinePath](find-osdcloudofflinepath.md) | No synopsis provided.                                             |
| [Find-TextInFile](find-textinfile.md)                   | Searches files for matching text and displays selectable results. |
| [Find-TextInModule](find-textinmodule.md)               | Searches module files for matching text.                          |

### Get Functions

| Function                                                                              | Description                                                                                            |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [Get-AzClipboard](get-azclipboard.md)                                                 | Read a secret value from the Azure clipboard Key Vault.                                                |
| [Get-AzOSDCloud](get-azosdcloud.md)                                                   | Initialize the local OSDCloud Azure workspace.                                                         |
| [Get-AzOSDTechId](get-azosdtechid.md)                                                 | Find Azure AD users for an OSD tech identifier prefix.                                                 |
| [Get-CimVideoControllerResolution](get-cimvideocontrollerresolution.md)               | Returns CIM video controller resolution entries for the system display adapter.                        |
| [Get-CloudSecret](get-cloudsecret.md)                                                 | Read a secret from Azure Key Vault.                                                                    |
| [Get-ComObjects](get-comobjects.md)                                                   | Lists registered COM ProgIDs from the local machine registry.                                          |
| [Get-ComObjMicrosoftUpdateAutoUpdate](get-comobjmicrosoftupdateautoupdate.md)         | Gets Microsoft Update automatic update settings through COM.                                           |
| [Get-ComObjMicrosoftUpdateInstaller](get-comobjmicrosoftupdateinstaller.md)           | Creates and returns the Microsoft Update installer COM object.                                         |
| [Get-ComObjMicrosoftUpdateServiceManager](get-comobjmicrosoftupdateservicemanager.md) | Gets Windows Update service registration details through COM.                                          |
| [Get-DataDisk](get-datadisk.md)                                                       | Gets DataDisk information.                                                                             |
| [Get-DellApplicationCatalog](get-dellapplicationcatalog.md)                           | Returns the Application Component of the Dell System Catalog                                           |
| [Get-DellBiosCatalog](get-dellbioscatalog.md)                                         | Returns the BIOS Component of the Dell System Catalog                                                  |
| [Get-DellDriverCatalog](get-delldrivercatalog.md)                                     | Returns the Driver Component of the Dell System Catalog                                                |
| [Get-DellDriverPackCatalog](get-delldriverpackcatalog.md)                             | Returns the Dell DriverPack Catalog                                                                    |
| [Get-DellFirmwareCatalog](get-dellfirmwarecatalog.md)                                 | Returns the Firmware Component of the Dell System Catalog                                              |
| [Get-DellSystemCatalog](get-dellsystemcatalog.md)                                     | Builds the Dell System Catalog                                                                         |
| [Get-DellWinPE10DriverPack](get-dellwinpe10driverpack.md)                             | Returns the URL of the latest Dell WinPE 10 Driver Pack                                                |
| [Get-DellWinPEDriverPack](get-dellwinpedriverpack.md)                                 | Returns the URL of the latest Dell WinPE 11 Driver Pack                                                |
| [Get-DisplayAllScreens](get-displayallscreens.md)                                     | Returns all display screens on the system                                                              |
| [Get-DisplayPrimaryBitmapSize](get-displayprimarybitmapsize.md)                       | Returns the primary display bitmap size accounting for DPI scaling                                     |
| [Get-DisplayPrimaryMonitorSize](get-displayprimarymonitorsize.md)                     | Returns the primary display monitor size in pixels                                                     |
| [Get-DisplayPrimaryScaling](get-displayprimaryscaling.md)                             | Returns the DPI scaling percentage of the primary display                                              |
| [Get-DisplayVirtualScreen](get-displayvirtualscreen.md)                               | Returns the virtual screen dimensions covering all displays                                            |
| [Get-DownLinks](get-downlinks.md)                                                     | Gets a list of links to download                                                                       |
| [Get-EnablementPackage](get-enablementpackage.md)                                     | Returns the latest matching Windows enablement package metadata.                                       |
| [Get-FeatureUpdate](get-featureupdate.md)                                             | Returns the latest matching Windows client feature update record.                                      |
| [Get-GithubRawContent](get-githubrawcontent.md)                                       | Retrieves content from GitHub or Gist raw URLs.                                                        |
| [Get-GithubRawUrl](get-githubrawurl.md)                                               | Resolves a GitHub or Gist URL to one or more raw content URLs.                                         |
| [Get-HPAccessoryCatalog](get-hpaccessorycatalog.md)                                   | Returns the 'Accessories Firmware and Driver' Component of the HP System Catalog                       |
| [Get-HPBiosCatalog](get-hpbioscatalog.md)                                             | Returns the BIOS Component of the HP System Catalog                                                    |
| [Get-HPDeviceFamilyPlatformDetails](get-hpdevicefamilyplatformdetails.md)             | No synopsis provided.                                                                                  |
| [Get-HPDriverCatalog](get-hpdrivercatalog.md)                                         | Returns the Driver Component of the HP System Catalog                                                  |
| [Get-HPDriverPackCatalog](get-hpdriverpackcatalog.md)                                 | Returns the HP DriverPack Catalog                                                                      |
| [Get-HPDriverPackLatest](get-hpdriverpacklatest.md)                                   | Gets the latest available HP driver pack for a platform.                                               |
| [Get-HPFirmwareCatalog](get-hpfirmwarecatalog.md)                                     | Returns the Firmware Component of the HP System Catalog                                                |
| [Get-HPIAJSONResult](get-hpiajsonresult.md)                                           | No synopsis provided.                                                                                  |
| [Get-HPIALatestVersion](get-hpialatestversion.md)                                     | No synopsis provided.                                                                                  |
| [Get-HPIAXMLResult](get-hpiaxmlresult.md)                                             | No synopsis provided.                                                                                  |
| [Get-HPOSSupport](get-hpossupport.md)                                                 | Gets supported Windows releases for an HP platform from the HPIA catalog.                              |
| [Get-HPPlatformCatalog](get-hpplatformcatalog.md)                                     | Converts the HP Platform list to a PowerShell Object.                                                  |
| [Get-HPSoftPaqItems](get-hpsoftpaqitems.md)                                           | Gets HPIA SoftPaq items for a specific HP platform and OS release.                                     |
| [Get-HPSoftpaqListLatest](get-hpsoftpaqlistlatest.md)                                 | Gets the latest HPIA SoftPaq list for an HP platform.                                                  |
| [Get-HPSoftwareCatalog](get-hpsoftwarecatalog.md)                                     | Returns the Software Component of the HP System Catalog                                                |
| [Get-HPSystemCatalog](get-hpsystemcatalog.md)                                         | Converts the HP Client Catalog for Microsoft System Center Product to a PowerShell Object              |
| [Get-HPTPMDetermine](get-hptpmdetermine.md)                                           | Determines which HP TPM firmware update package is required for the current device.                    |
| [Get-HpWinPEDriverPack](get-hpwinpedriverpack.md)                                     | Returns the URL of the latest HP WinPE 10 Driver Pack                                                  |
| [Get-HyperVName](get-hypervname.md)                                                   | No synopsis provided.                                                                                  |
| [Get-IntelEthernetDriverPack](get-intelethernetdriverpack.md)                         | Returns the Intel Ethernet Driver Object                                                               |
| [Get-IntelGraphicsDriverPack](get-intelgraphicsdriverpack.md)                         | Returns the Intel Graphics Driver Object                                                               |
| [Get-IntelRadeonDriverPack](get-intelradeondriverpack.md)                             | Returns the Intel Radeon Graphics Driver Object                                                        |
| [Get-IntelWirelessDriverPack](get-intelwirelessdriverpack.md)                         | Returns the Intel Wireless Driver Object                                                               |
| [Get-LenovoBiosCatalog](get-lenovobioscatalog.md)                                     | Builds the Lenovo Bios Catalog                                                                         |
| [Get-LenovoDriverPackCatalog](get-lenovodriverpackcatalog.md)                         | Returns the Lenovo DriverPack Catalog                                                                  |
| [Get-LocalDisk](get-localdisk.md)                                                     | Gets LocalDisk information.                                                                            |
| [Get-LocalDiskPartition](get-localdiskpartition.md)                                   | Gets LocalDiskPartition information.                                                                   |
| [Get-LocalDiskVolume](get-localdiskvolume.md)                                         | Gets LocalDiskVolume information.                                                                      |
| [Get-MsUpCat](get-msupcat.md)                                                         | Retrieves Microsoft updates from the Microsoft Update Catalog                                          |
| [Get-MsUpCatUpdate](get-msupcatupdate.md)                                             | Retrieves updates for a specific Windows operating system version from Microsoft Update Catalog        |
| [Get-MyBiosSerialNumber](get-mybiosserialnumber.md)                                   | Gets MyBiosSerialNumber information.                                                                   |
| [Get-MyBiosUpdate](get-mybiosupdate.md)                                               | Gets MyBiosUpdate information.                                                                         |
| [Get-MyBiosVersion](get-mybiosversion.md)                                             | Gets MyBiosVersion information.                                                                        |
| [Get-MyBitLockerKeyProtectors](get-mybitlockerkeyprotectors.md)                       | Returns BitLocker key protector details for encrypted volumes.                                         |
| [Get-MyComputerManufacturer](get-mycomputermanufacturer.md)                           | Gets MyComputerManufacturer information.                                                               |
| [Get-MyComputerModel](get-mycomputermodel.md)                                         | Gets MyComputerModel information.                                                                      |
| [Get-MyComputerProduct](get-mycomputerproduct.md)                                     | Gets MyComputerProduct information.                                                                    |
| [Get-MyDefaultAUService](get-mydefaultauservice.md)                                   | Returns the Default AU Service from Microsoft.Update.ServiceManager                                    |
| [Get-MyDellBios](get-mydellbios.md)                                                   | Returns the latest compatible Dell BIOS update for the current system.                                 |
| [Get-MyDriverPack](get-mydriverpack.md)                                               | Retrieves the driver pack for the current computer from OSDCloud                                       |
| [Get-MyWindowsCapability](get-mywindowscapability.md)                                 | Gets MyWindowsCapability information.                                                                  |
| [Get-MyWindowsPackage](get-mywindowspackage.md)                                       | Gets MyWindowsPackage information.                                                                     |
| [Get-NativeMatchineImage](get-nativematchineimage.md)                                 | Gets NativeMatchineImage information.                                                                  |
| [Get-OSD](get-osd.md)                                                                 | Displays information about the OSD Module                                                              |
| [Get-OSDClass](get-osdclass.md)                                                       | Returns CimInstance information from common OSD Classes                                                |
| [Get-OSDCloudAzureResources](get-osdcloudazureresources.md)                           | Discover OSDCloud Azure Storage resources.                                                             |
| [Get-OSDCloudDriverPack](get-osdclouddriverpack.md)                                   | Gets the OSDCloud DriverPack for the current or specified computer model                               |
| [Get-OSDCloudDriverPacks](get-osdclouddriverpacks.md)                                 | Returns the DriverPacks used by OSDCloud                                                               |
| [Get-OSDCloudOperatingSystems](get-osdcloudoperatingsystems.md)                       | Gets OSDCloud operating system entries for a specific architecture.                                    |
| [Get-OSDCloudOperatingSystemsIndexes](get-osdcloudoperatingsystemsindexes.md)         | Returns OSDCloud operating system index entries by architecture.                                       |
| [Get-OSDCloudOperatingSystemsIndexMap](get-osdcloudoperatingsystemsindexmap.md)       | Returns OSDCloud operating system index map entries by architecture.                                   |
| [Get-OSDCloudOSNames](get-osdcloudosnames.md)                                         | Returns the Operating Systems names used by OSDCloud                                                   |
| [Get-OSDCloudTemplate](get-osdcloudtemplate.md)                                       | Gets information returned by Get-OSDCloudTemplate.                                                     |
| [Get-OSDCloudTemplateNames](get-osdcloudtemplatenames.md)                             | Gets information returned by Get-OSDCloudTemplateNames.                                                |
| [Get-OSDCloudVMDefaults](get-osdcloudvmdefaults.md)                                   | Gets the OSDCloudVM Module defaults from $Global:OSDModuleResource.NewOSDCloudVM                       |
| [Get-OSDCloudVMSettings](get-osdcloudvmsettings.md)                                   | Gets information returned by Get-OSDCloudVMSettings.                                                   |
| [Get-OSDCloudWorkspace](get-osdcloudworkspace.md)                                     | Gets information returned by Get-OSDCloudWorkspace.                                                    |
| [Get-OSDCoreCacheContent](get-osdcorecachecontent.md)                                 | Returns cached OSDCloud content found on local file system drives.                                     |
| [Get-OSDCoreCacheDrive](get-osdcorecachedrive.md)                                     | Returns OSDCloud cache drive metadata from local file system drives.                                   |
| [Get-OSDCoreCacheUSBPath](get-osdcorecacheusbpath.md)                                 | Returns OSDCloud cache paths located on USB drives.                                                    |
| [Get-OSDCoreDeploymentDisk](get-osdcoredeploymentdisk.md)                             | Retrieves disk objects suitable for OS deployment with enhanced filtering capabilities.                |
| [Get-OSDCoreDriverPackCatalogDell](get-osdcoredriverpackcatalogdell.md)               | Downloads and parses the Dell driver pack catalog for Windows 11.                                      |
| [Get-OSDCoreDriverPackCatalogHP](get-osdcoredriverpackcataloghp.md)                   | Downloads and parses the HP driver pack catalog for Windows 11.                                        |
| [Get-OSDCoreDriverPackCatalogLenovo](get-osdcoredriverpackcataloglenovo.md)           | Downloads and parses the Lenovo driver pack catalog for Windows 11.                                    |
| [Get-OSDCoreDriverPackCatalogPanasonic](get-osdcoredriverpackcatalogpanasonic.md)     | No synopsis provided.                                                                                  |
| [Get-OSDCoreDriverPackCatalogSurface](get-osdcoredriverpackcatalogsurface.md)         | Retrieves the Microsoft Surface driver pack catalog, enriching entries from live download pages.       |
| [Get-OSDCoreDriverPacks](get-osdcoredriverpacks.md)                                   | Retrieves driver pack information for the specified manufacturer and operating system architecture.    |
| [Get-OSDCoreLicense](get-osdcorelicense.md)                                           | Returns a single Recast Core license object.                                                           |
| [Get-OSDCoreOperatingSystems](get-osdcoreoperatingsystems.md)                         | Gets the core operating system catalog entries that OSD uses for offline media selection.              |
| [Get-OSDDisk](get-osddisk.md)                                                         | Gets OSDDisk information.                                                                              |
| [Get-OSDGather](get-osdgather.md)                                                     | Returns common OSD information as an ordered hash table                                                |
| [Get-OSDHelp](get-osdhelp.md)                                                         | Gets OSDHelp information.                                                                              |
| [Get-OSDMetrics](get-osdmetrics.md)                                                   | Retrieves metrics for the OSD PowerShell module and OSDCloud deployment methods.                       |
| [Get-OSDModuleCache](get-osdmodulecache.md)                                           | Returns the OSD module cache directory path.                                                           |
| [Get-OSDModulePath](get-osdmodulepath.md)                                             | Returns the base path of the loaded OSD module.                                                        |
| [Get-OSDModuleVersion](get-osdmoduleversion.md)                                       | Returns the version of the loaded OSD module.                                                          |
| [Get-OSDPad](get-osdpad.md)                                                           | Gets information returned by Get-OSDPad.                                                               |
| [Get-OSDPartition](get-osdpartition.md)                                               | Gets OSDPartition information.                                                                         |
| [Get-OSDPower](get-osdpower.md)                                                       | Displays Power Plan information using powercfg /LIST                                                   |
| [Get-OSDVolume](get-osdvolume.md)                                                     | Gets OSDVolume information.                                                                            |
| [Get-OSDWinEvent](get-osdwinevent.md)                                                 | Gets OSDWinEvent information.                                                                          |
| [Get-PowerSettingSleepAfter](get-powersettingsleepafter.md)                           | No synopsis provided.                                                                                  |
| [Get-PowerSettingTurnMonitorOffAfter](get-powersettingturnmonitoroffafter.md)         | Gets the active power plan monitor-off timeout in minutes.                                             |
| [Get-PSCloudScript](get-pscloudscript.md)                                             | Development function to get the contents of a PSCloudScript.                                           |
| [Get-ReAgentXml](get-reagentxml.md)                                                   | Returns information from the Windows Recovery Agent XML file                                           |
| [Get-RegCurrentVersion](get-regcurrentversion.md)                                     | Returns the Registry Key values from HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion                |
| [Get-ScreenPNG](get-screenpng.md)                                                     | Gets ScreenPNG information.                                                                            |
| [Get-SessionsXml](get-sessionsxml.md)                                                 | Returns the Session.xml Updates that have been applied to an Operating System                          |
| [Get-SetupCompleteOSDCloudUSB](get-setupcompleteosdcloudusb.md)                       | This function checks for the presence of an OSDCloud SetupComplete Folder on any drive other than 'C'. |
| [Get-SurfaceDriverPackCatalog](get-surfacedriverpackcatalog.md)                       | Returns the Microsoft Surface DriverPack Catalog                                                       |
| [Get-SystemFirmwareDevice](get-systemfirmwaredevice.md)                               | Returns the system firmware device                                                                     |
| [Get-SystemFirmwareResource](get-systemfirmwareresource.md)                           | Returns the GUID of the system firmware resource                                                       |
| [Get-SystemFirmwareUpdate](get-systemfirmwareupdate.md)                               | Retrieves the latest system firmware update from Microsoft Update Catalog                              |
| [Get-TimeZoneFromIP](get-timezonefromip.md)                                           | No synopsis provided.                                                                                  |
| [Get-USBDisk](get-usbdisk.md)                                                         | Gets USBDisk information.                                                                              |
| [Get-USBPartition](get-usbpartition.md)                                               | Gets USBPartition information.                                                                         |
| [Get-USBVolume](get-usbvolume.md)                                                     | Gets USBVolume information.                                                                            |
| [Get-WiFiActiveProfileSSID](get-wifiactiveprofilessid.md)                             | No synopsis provided.                                                                                  |
| [Get-WiFiProfileKey](get-wifiprofilekey.md)                                           | No synopsis provided.                                                                                  |
| [Get-Win11Readiness](get-win11readiness.md)                                           | No synopsis provided.                                                                                  |
| [Get-WindowsAdkInstallPath](get-windowsadkinstallpath.md)                             | Retrieves the installation path of the Windows Assessment and Deployment Kit (ADK)                     |
| [Get-WindowsAdkInstallVersion](get-windowsadkinstallversion.md)                       | Retrieves the installed version of the Windows Assessment and Deployment Kit (ADK)                     |
| [Get-WindowsAdkPaths](get-windowsadkpaths.md)                                         | Retrieves the command paths of the Windows Assessment and Deployment Kit (ADK).                        |
| [Get-WindowsKitsInstallPath](get-windowskitsinstallpath.md)                           | Retrieves the installation path of the Windows Kit directory.                                          |
| [Get-WindowsOEMProductKey](get-windowsoemproductkey.md)                               | No synopsis provided.                                                                                  |
| [Get-WindowsUpdateDriver](get-windowsupdatedriver.md)                                 | No synopsis provided.                                                                                  |
| [Get-WindowsUpdateManifests](get-windowsupdatemanifests.md)                           | Returns an Array of Microsoft Updates from the Microsoft Update Catalog                                |
| [Get-WinREPartition](get-winrepartition.md)                                           | Retrieves the Windows Recovery Environment partition information                                       |
| [Get-WSUSXML](get-wsusxml.md)                                                         | Returns an Array of Microsoft Updates                                                                  |

### Import Functions

| Function                                                          | Description                                                |
| ----------------------------------------------------------------- | ---------------------------------------------------------- |
| [Import-MDTWinPECloudDriver](import-mdtwinpeclouddriver.md)       | Imports OSDCloud CloudDrivers into an MDT Deployment Share |
| [Import-OSDCloudWinPEDriverMDT](import-osdcloudwinpedrivermdt.md) | Imports OSDCloud CloudDrivers into an MDT Deployment Share |

### Initialize Functions

| Function                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| [Initialize-OSDCloudStartnet](initialize-osdcloudstartnet.md)             | Initializes the OSDCloud startnet environment.                            |
| [Initialize-OSDCloudStartnetUpdate](initialize-osdcloudstartnetupdate.md) | No synopsis provided.                                                     |
| [Initialize-OSDCoreDevice](initialize-osdcoredevice.md)                   | Collects local hardware, firmware, TPM, and network details for OSDCloud. |

### Install Functions

| Function                                                                    | Description                                                                             |
| --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| [Install-AzOSDIacTools](install-azosdiactools.md)                           | Install prerequisite IaC tooling for OSDCloud Azure.                                    |
| [Install-BuildUpdatesFromOSCloudUSB](install-buildupdatesfromoscloudusb.md) | No synopsis provided.                                                                   |
| [Install-HPIA](install-hpia.md)                                             | No synopsis provided.                                                                   |
| [Install-ModuleHPCMSL](install-modulehpcmsl.md)                             | Installs or updates the HP Client Management Script Library (HPCMSL) PowerShell module. |
| [Install-SystemFirmwareUpdate](install-systemfirmwareupdate.md)             | Downloads and installs the system firmware update                                       |

### Invoke Functions

| Function                                                          | Description                                                                      |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| [Invoke-AzOSDAzureConfig](invoke-azosdazureconfig.md)             | Deploy OSDCloud Azure infrastructure with Bicep or Terraform.                    |
| [Invoke-CloudSecret](invoke-cloudsecret.md)                       | Invoke a secret retrieved from Azure Key Vault.                                  |
| [Invoke-Exe](invoke-exe.md)                                       | Runs an external command.                                                        |
| [Invoke-HPAnalyzer](invoke-hpanalyzer.md)                         | No synopsis provided.                                                            |
| [Invoke-HPDriverUpdate](invoke-hpdriverupdate.md)                 | No synopsis provided.                                                            |
| [Invoke-HPIA](invoke-hpia.md)                                     | No synopsis provided.                                                            |
| [Invoke-HPIAOfflineSync](invoke-hpiaofflinesync.md)               | Creates and synchronizes an offline HPIA repository for the local HP platform.   |
| [Invoke-HPTPMDowngrade](invoke-hptpmdowngrade.md)                 | Downloads and applies the HP SP94937 softpaq to downgrade a TPM from 2.0 to 1.2. |
| [Invoke-HPTPMDownload](invoke-hptpmdownload.md)                   | Downloads and extracts the required HP TPM firmware update softpaq using HPCMSL. |
| [Invoke-HPTPMEXEDownload](invoke-hptpmexedownload.md)             | Downloads the required HP TPM firmware EXE to C:\OSDCloud\HP\TPM.                |
| [Invoke-HPTPMEXEInstall](invoke-hptpmexeinstall.md)               | Extracts and installs the HP TPM firmware update from C:\OSDCloud\HP\TPM.        |
| [Invoke-MSCatalogParseDate](invoke-mscatalogparsedate.md)         | Parses a date string from Microsoft Update Catalog format                        |
| [Invoke-OSDCloud](invoke-osdcloud.md)                             | This is the master OSDCloud Task Sequence                                        |
| [Invoke-OSDCloudDriverPackCM](invoke-osdclouddriverpackcm.md)     | Downloads a matching DriverPack to %OSDisk%\Drivers                              |
| [Invoke-OSDCloudDriverPackMDT](invoke-osdclouddriverpackmdt.md)   | Downloads a matching DriverPack to %OSDisk%\Drivers                              |
| [Invoke-OSDCloudDriverPackPPKG](invoke-osdclouddriverpackppkg.md) | Uses DISM in WinPE to expand and apply Driver Packs                              |
| [Invoke-OSDCloudIPU](invoke-osdcloudipu.md)                       | Starts an OSDCloud in-place upgrade workflow.                                    |
| [Invoke-OSDInfo](invoke-osdinfo.md)                               | Displays OSD information, useful in an OS Deployment                             |
| [Invoke-OSDSpecialize](invoke-osdspecialize.md)                   | No synopsis provided.                                                            |
| [Invoke-OSDSpecializeDev](invoke-osdspecializedev.md)             | No synopsis provided.                                                            |
| [Invoke-SelectDataDisk](invoke-selectdatadisk.md)                 | Invokes SelectDataDisk actions.                                                  |
| [Invoke-SelectFFUDisk](invoke-selectffudisk.md)                   | Invokes SelectFFUDisk actions.                                                   |
| [Invoke-SelectLocalDisk](invoke-selectlocaldisk.md)               | Invokes SelectLocalDisk actions.                                                 |
| [Invoke-SelectLocalVolume](invoke-selectlocalvolume.md)           | Invokes SelectLocalVolume actions.                                               |
| [Invoke-SelectOSDDisk](invoke-selectosddisk.md)                   | Invokes SelectOSDDisk actions.                                                   |
| [Invoke-SelectOSDVolume](invoke-selectosdvolume.md)               | Invokes SelectOSDVolume actions.                                                 |
| [Invoke-SelectUSBDisk](invoke-selectusbdisk.md)                   | Invokes SelectUSBDisk actions.                                                   |
| [Invoke-SelectUSBVolume](invoke-selectusbvolume.md)               | Invokes SelectUSBVolume actions.                                                 |
| [Invoke-WebPSScript](invoke-webpsscript.md)                       | Executes a PowerShell script from a URL.                                         |

### Mount Functions

| Function                                        | Description                          |
| ----------------------------------------------- | ------------------------------------ |
| [Mount-MyWindowsImage](mount-mywindowsimage.md) | Mounts MyWindowsImage for servicing. |

### New Functions

| Function                                                                                    | Description                                                                 |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [New-AdkCopyPE](new-adkcopype.md)                                                           | Creates an ADK CopyPE working directory                                     |
| [New-AdkISO](new-adkiso.md)                                                                 | Creates an ISO file from a bootable media directory using ADK tools         |
| [New-BootableUSBDrive](new-bootableusbdrive.md)                                             | Creates BootableUSBDrive resources.                                         |
| [New-CAB](new-cab.md)                                                                       | Creates a CAB file from a Directory                                         |
| [New-OSDCloudISO](new-osdcloudiso.md)                                                       | Creates an OSDCloud bootable ISO from an OSDCloud workspace.                |
| [New-OSDCloudOSWimFile](new-osdcloudoswimfile.md)                                           | Builds Windows setup media content for an OSDCloud feature update.          |
| [New-OSDCloudTemplate](new-osdcloudtemplate.md)                                             | Creates resources by using New-OSDCloudTemplate.                            |
| [New-OSDCloudUSB](new-osdcloudusb.md)                                                       | Creates resources by using New-OSDCloudUSB.                                 |
| [New-OSDCloudUSBSetupCompleteTemplate](new-osdcloudusbsetupcompletetemplate.md)             | Creates resources by using New-OSDCloudUSBSetupCompleteTemplate.            |
| [New-OSDCloudVM](new-osdcloudvm.md)                                                         | Creates resources by using New-OSDCloudVM.                                  |
| [New-OSDCloudWorkspace](new-osdcloudworkspace.md)                                           | Creates resources by using New-OSDCloudWorkspace.                           |
| [New-OSDCloudWorkspaceSetupCompleteTemplate](new-osdcloudworkspacesetupcompletetemplate.md) | Creates resources by using New-OSDCloudWorkspaceSetupCompleteTemplate.      |
| [New-OSDisk](new-osdisk.md)                                                                 | Creates System \| OS \| Recovery Partitions for MBR or UEFI Drives in WinPE |
| [New-WindowsAdkISO](new-windowsadkiso.md)                                                   | Creates an ISO file from a bootable media directory using ADK               |

### Remove Functions

| Function                                  | Description               |
| ----------------------------------------- | ------------------------- |
| [Remove-AppxOnline](remove-appxonline.md) | Removes AppxOnline items. |

### Reset Functions

| Function                                                | Description                                             |
| ------------------------------------------------------- | ------------------------------------------------------- |
| [Reset-OSDCloudVMSettings](reset-osdcloudvmsettings.md) | Resets configuration by using Reset-OSDCloudVMSettings. |

### Resolve Functions

| Function                          | Description                                      |
| --------------------------------- | ------------------------------------------------ |
| [Resolve-MsUrl](resolve-msurl.md) | Resolves a short Microsoft aka.ms or fwlink URL. |

### Save Functions

| Function                                                                | Description                                                                    |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [Save-ClipboardImage](save-clipboardimage.md)                           | Saves ClipboardImage content.                                                  |
| [Save-EnablementPackage](save-enablementpackage.md)                     | Downloads a matching Windows enablement package.                               |
| [Save-FeatureUpdate](save-featureupdate.md)                             | Downloads the latest matching Windows client feature update package.           |
| [Save-MsUpCatDriver](save-msupcatdriver.md)                             | Downloads driver updates from Microsoft Update Catalog                         |
| [Save-MsUpCatUpdate](save-msupcatupdate.md)                             | Downloads updates from Microsoft Update Catalog for a specific Windows version |
| [Save-MyBitLockerExternalKey](save-mybitlockerexternalkey.md)           | Saves BitLocker external key protectors (.BEK) to destination folders.         |
| [Save-MyBitLockerKeyPackage](save-mybitlockerkeypackage.md)             | Saves BitLocker key packages to destination folders.                           |
| [Save-MyBitLockerRecoveryPassword](save-mybitlockerrecoverypassword.md) | Saves BitLocker recovery passwords to text files.                              |
| [Save-MyDellBios](save-mydellbios.md)                                   | Downloads the latest compatible Dell BIOS update to a local folder.            |
| [Save-MyDellBiosFlash64W](save-mydellbiosflash64w.md)                   | Downloads and extracts the Dell Flash64W BIOS utility.                         |
| [Save-MyDriverPack](save-mydriverpack.md)                               | Downloads and optionally expands the driver pack for the current computer      |
| [Save-SystemFirmwareUpdate](save-systemfirmwareupdate.md)               | Downloads and extracts the latest system firmware update.                      |
| [Save-WebFile](save-webfile.md)                                         | Downloads a file from the internet and returns a Get-Item Object               |
| [Save-WinPECloudDriver](save-winpeclouddriver.md)                       | Download and expand WinPE Drivers                                              |
| [Save-ZTIDriverPack](save-ztidriverpack.md)                             | Downloads the driver pack for a computer during MDT/ConfigMgr task sequence    |

### Select Functions

| Function                                                                | Description                           |
| ----------------------------------------------------------------------- | ------------------------------------- |
| [Select-OSDCloudAutopilotJsonItem](select-osdcloudautopilotjsonitem.md) | Selects Autopilot Profiles            |
| [Select-OSDCloudFileWim](select-osdcloudfilewim.md)                     | Selects Office Configuration Profiles |
| [Select-OSDCloudImageIndex](select-osdcloudimageindex.md)               | No synopsis provided.                 |
| [Select-OSDCloudODTFile](select-osdcloudodtfile.md)                     | Selects Office Configuration Profiles |

### Set Functions

| Function                                                                                  | Description                                                                                        |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| [Set-AzClipboard](set-azclipboard.md)                                                     | Write the current clipboard text to the Azure clipboard Key Vault.                                 |
| [Set-BitlockerRegValuesXTS256](set-bitlockerregvaluesxts256.md)                           | No synopsis provided.                                                                              |
| [Set-BootmgrTimeout](set-bootmgrtimeout.md)                                               | Sets the Windows Boot Manager timeout value in BCD.                                                |
| [Set-ClipboardScreenshot](set-clipboardscreenshot.md)                                     | Captures a screenshot and copies it to the clipboard                                               |
| [Set-CloudSecret](set-cloudsecret.md)                                                     | Convert content to an Azure Key Vault secret.                                                      |
| [Set-DisRes](set-disres.md)                                                               | Sets the primary display screen resolution.                                                        |
| [Set-HPBIOSSetting](set-hpbiossetting.md)                                                 | No synopsis provided.                                                                              |
| [Set-HPTPMBIOSSettings](set-hptpmbiossettings.md)                                         | No synopsis provided.                                                                              |
| [Set-HyperVName](set-hypervname.md)                                                       | No synopsis provided.                                                                              |
| [Set-LatestUpdatesASAPEnabled](set-latestupdatesasapenabled.md)                           | No synopsis provided.                                                                              |
| [Set-OSDCloudTemplate](set-osdcloudtemplate.md)                                           | Sets configuration values by using Set-OSDCloudTemplate.                                           |
| [Set-OSDCloudUnattendAuditMode](set-osdcloudunattendauditmode.md)                         | No synopsis provided.                                                                              |
| [Set-OSDCloudUnattendAuditModeAutopilot](set-osdcloudunattendauditmodeautopilot.md)       | No synopsis provided.                                                                              |
| [Set-OSDCloudUnattendSpecialize](set-osdcloudunattendspecialize.md)                       | No synopsis provided.                                                                              |
| [Set-OSDCloudUnattendSpecializeDev](set-osdcloudunattendspecializedev.md)                 | No synopsis provided.                                                                              |
| [Set-OSDCloudVMSettings](set-osdcloudvmsettings.md)                                       | Sets configuration values by using Set-OSDCloudVMSettings.                                         |
| [Set-OSDCloudWorkspace](set-osdcloudworkspace.md)                                         | Sets configuration values by using Set-OSDCloudWorkspace.                                          |
| [Set-OSDxCloudUnattendSpecialize](set-osdxcloudunattendspecialize.md)                     | No synopsis provided.                                                                              |
| [Set-PowerSettingSleepAfter](set-powersettingsleepafter.md)                               | No synopsis provided.                                                                              |
| [Set-PowerSettingTurnMonitorOffAfter](set-powersettingturnmonitoroffafter.md)             | No synopsis provided.                                                                              |
| [Set-SetupCompleteBitlocker](set-setupcompletebitlocker.md)                               | No synopsis provided.                                                                              |
| [Set-SetupCompleteCreateFinish](set-setupcompletecreatefinish.md)                         | No synopsis provided.                                                                              |
| [Set-SetupCompleteCreateStart](set-setupcompletecreatestart.md)                           | No synopsis provided.                                                                              |
| [Set-SetupCompleteDefenderUpdate](set-setupcompletedefenderupdate.md)                     | No synopsis provided.                                                                              |
| [Set-SetupCompleteHPAppend](set-setupcompletehpappend.md)                                 | No synopsis provided.                                                                              |
| [Set-SetupCompleteHyperVName](set-setupcompletehypervname.md)                             | No synopsis provided.                                                                              |
| [Set-SetupCompleteNetFX](set-setupcompletenetfx.md)                                       | No synopsis provided.                                                                              |
| [Set-SetupCompleteOEMActivation](set-setupcompleteoemactivation.md)                       | No synopsis provided.                                                                              |
| [Set-SetupCompleteOSDCloudCustom](set-setupcompleteosdcloudcustom.md)                     | No synopsis provided.                                                                              |
| [Set-SetupCompleteOSDCloudUSB](set-setupcompleteosdcloudusb.md)                           | This function copies SetupComplete Files to the Local OSDCloud SetupComplete Folder                |
| [Set-SetupCompleteSetWiFi](set-setupcompletesetwifi.md)                                   | No synopsis provided.                                                                              |
| [Set-SetupCompleteStartWindowsUpdate](set-setupcompletestartwindowsupdate.md)             | No synopsis provided.                                                                              |
| [Set-SetupCompleteStartWindowsUpdateDriver](set-setupcompletestartwindowsupdatedriver.md) | No synopsis provided.                                                                              |
| [Set-SetupCompleteTimeZone](set-setupcompletetimezone.md)                                 | No synopsis provided.                                                                              |
| [Set-TimeZoneFromIP](set-timezonefromip.md)                                               | No synopsis provided.                                                                              |
| [Set-WiFi](set-wifi.md)                                                                   | No synopsis provided.                                                                              |
| [Set-WimExecutionPolicy](set-wimexecutionpolicy.md)                                       | Sets the PowerShell Execution Policy of a Windows Image .wim file (Mount \| Set \| Dismount -Save) |
| [Set-WindowsImageExecutionPolicy](set-windowsimageexecutionpolicy.md)                     | Sets the PowerShell Execution Policy of a mounted Windows Image                                    |
| [Set-WindowsOEMActivation](set-windowsoemactivation.md)                                   | No synopsis provided.                                                                              |

### Show Functions

| Function                                              | Description                                                                  |
| ----------------------------------------------------- | ---------------------------------------------------------------------------- |
| [Show-MsSettings](show-mssettings.md)                 | Opens the ms-setting: URI that is specified in the Setting parameter         |
| [Show-OSDCoreLicenseHelp](show-osdcorelicensehelp.md) | Displays instructions for setting the Recast Core license for OSDCloud.      |
| [Show-RegistryXML](show-registryxml.md)               | Displays registry entries from all RegistryXML files in the Source Directory |

### Start Functions

| Function                                                  | Description                                                                                            |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [Start-DiskImageGUI](start-diskimagegui.md)               | Start-DiskImageGUI function.                                                                           |
| [Start-DISMFromOSDCloudUSB](start-dismfromosdcloudusb.md) | No synopsis provided.                                                                                  |
| [Start-EjectCD](start-ejectcd.md)                         | No synopsis provided.                                                                                  |
| [Start-OOBEDeploy](start-oobedeploy.md)                   | No synopsis provided.                                                                                  |
| [Start-OSDCloud](start-osdcloud.md)                       | Prepare and start an OSDCloud deployment session (selects image, language, edition and other options). |
| [Start-OSDCloudAzure](start-osdcloudazure.md)             | Start an OSDCloud deployment from Azure Storage.                                                       |
| [Start-OSDCloudCLI](start-osdcloudcli.md)                 | Starts the OSDCloud Windows 10 or 11 Build Process from the OSD Module or a GitHub Repository          |
| [Start-OSDCloudGUI](start-osdcloudgui.md)                 | OSDCloud imaging using the command line                                                                |
| [Start-OSDCloudGUIDev](start-osdcloudguidev.md)           | OSDCloud imaging using the command line                                                                |
| [Start-OSDCloudREAzure](start-osdcloudreazure.md)         | OSDCloudRE: Creates a new OSDCloudRE Volume from Azure                                                 |
| [Start-OSDCloudToolbox](start-osdcloudtoolbox.md)         | Starts the workflow for Start-OSDCloudToolbox.                                                         |
| [Start-OSDDiskPart](start-osddiskpart.md)                 | No synopsis provided.                                                                                  |
| [Start-OSDeployPad](start-osdeploypad.md)                 | Starts the workflow for Start-OSDeployPad.                                                             |
| [Start-OSDPad](start-osdpad.md)                           | Starts the workflow for Start-OSDPad.                                                                  |
| [Start-OSDPadCategories](start-osdpadcategories.md)       | Starts the workflow for Start-OSDPadCategories.                                                        |
| [Start-RecastOSDCloudCLI](start-recastosdcloudcli.md)     | Starts the Recast OSDCloud command-line deployment workflow.                                           |
| [Start-ScreenPNGProcess](start-screenpngprocess.md)       | Starts a background process to capture screenshots                                                     |
| [Start-WindowsUpdate](start-windowsupdate.md)             | No synopsis provided.                                                                                  |
| [Start-WindowsUpdateDriver](start-windowsupdatedriver.md) | No synopsis provided.                                                                                  |

### Stop Functions

| Function                                          | Description                                     |
| ------------------------------------------------- | ----------------------------------------------- |
| [Stop-ScreenPNGProcess](stop-screenpngprocess.md) | Stops the background screenshot capture process |

### Test Functions

| Function                                                                            | Description                                                                               |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [Test-DCUSupport](test-dcusupport.md)                                               | No synopsis provided.                                                                     |
| [Test-DISMFromOSDCloudUSB](test-dismfromosdcloudusb.md)                             | No synopsis provided.                                                                     |
| [Test-DynamicValidateSet](test-dynamicvalidateset.md)                               | Tests DynamicValidateSet conditions.                                                      |
| [Test-HPIASupport](test-hpiasupport.md)                                             | Tests whether the current HP platform is supported by HPIA.                               |
| [Test-HPTPMFromOSDCloudUSB](test-hptpmfromosdcloudusb.md)                           | Tests whether HP TPM firmware packages exist on an OSDCloud USB drive.                    |
| [Test-IsVM](test-isvm.md)                                                           | Tests IsVM conditions.                                                                    |
| [Test-MicrosoftUpdateCatalog](test-microsoftupdatecatalog.md)                       | Tests connectivity to Microsoft Update Catalog.                                           |
| [Test-OSDCoreCacheUSB](test-osdcorecacheusb.md)                                     | Tests whether any OSDCloud cache drive is a USB drive.                                    |
| [Test-OSDCoreDriverPackCloudObject](test-osdcoredriverpackcloudobject.md)           | Tests whether an OSDCore driver pack object URL is reachable.                             |
| [Test-OSDCoreOperatingSystemCloudObject](test-osdcoreoperatingsystemcloudobject.md) | Tests whether an OSDCore operating system object URL is reachable.                        |
| [Test-WebConnection](test-webconnection.md)                                         | Tests web connectivity to a target URI using a live TCP connection and HTTP HEAD request. |
| [Test-WindowsImage](test-windowsimage.md)                                           | Tests WindowsImage conditions.                                                            |
| [Test-WindowsImageMounted](test-windowsimagemounted.md)                             | Tests WindowsImageMounted conditions.                                                     |
| [Test-WindowsImageMountPath](test-windowsimagemountpath.md)                         | Tests WindowsImageMountPath conditions.                                                   |
| [Test-WindowsPackageCAB](test-windowspackagecab.md)                                 | Tests WindowsPackageCAB conditions.                                                       |

### Unblock Functions

| Function                                          | Description                                            |
| ------------------------------------------------- | ------------------------------------------------------ |
| [Unblock-WindowsUpdate](unblock-windowsupdate.md) | Opens Windows Update and checks for WSUS configuration |

### Unlock Functions

| Function                                                          | Description                                         |
| ----------------------------------------------------------------- | --------------------------------------------------- |
| [Unlock-MyBitLockerExternalKey](unlock-mybitlockerexternalkey.md) | Unlocks BitLocker volumes using external key files. |

### Update Functions

| Function                                                          | Description                                                                  |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| [Update-DefenderStack](update-defenderstack.md)                   | No synopsis provided.                                                        |
| [Update-IntelDriversCatalog](update-inteldriverscatalog.md)       | Updates the Intel Drivers Cats in the OSD Module                             |
| [Update-MyDellBios](update-mydellbios.md)                         | Downloads and launches a compatible BIOS update for the current Dell system. |
| [Update-MyWindowsImage](update-mywindowsimage.md)                 | Updates MyWindowsImage content.                                              |
| [Update-OSDCloudUSB](update-osdcloudusb.md)                       | Updates resources by using Update-OSDCloudUSB.                               |
| [Update-RecastOSDCloudUSBCache](update-recastosdcloudusbcache.md) | Starts the Recast OSDCloud command-line deployment workflow.                 |

### Wait Functions

| Function                                    | Description                                           |
| ------------------------------------------- | ----------------------------------------------------- |
| [Wait-WebConnection](wait-webconnection.md) | Waits for an internet connection to the specified Uri |

### Write Functions

| Function                                | Description           |
| --------------------------------------- | --------------------- |
| [Write-CMTraceLog](write-cmtracelog.md) | No synopsis provided. |

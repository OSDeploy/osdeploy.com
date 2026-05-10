# PowerShell Modules

The following PowerShell modules are published to the [PowerShell Gallery](https://www.powershellgallery.com/) and are required for OSDeploy workflows. Install them before proceeding with any OSDeploy, OSDCloud, or MDT-based tasks.

Always use `-SkipPublisherCheck` when installing these modules. `-Force` allows reinstall or upgrade without a confirmation prompt.

***

## OSDeploy

The **OSDeploy** module is used on **Windows 11 25H2** to create WinPE boot images. It runs in a full Windows OS environment — not in WinPE.

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
```

See the [Functions](functions.md) page for the full list of available commands, or navigate directly to any function reference page in the sidebar.

***

## OSDCloud

The **OSDCloud** module is used **in WinPE** to deploy Windows 11 to a device. This is the current, recommended module for all OSDCloud deployments.

```powershell
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

***

## OSD

The **OSD** module is used **in WinPE** to deploy Windows 11. It also supports the older OSDCloud implementation, known as OSDCloud v1 (legacy).

```powershell
Install-Module -Name OSD -Force -SkipPublisherCheck
```

{% hint style="warning" %}
For OSDCloud functionality, use the **OSDCloud** module instead of OSD. The OSDCloud module supersedes the OSDCloud v1 implementation that existed in OSD. The OSD module is maintained, but the OSDCloud module is preferred.
{% endhint %}

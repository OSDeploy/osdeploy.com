---
description: >-
  Build customized OSDeploy WinPE media from an imported WinRE image or the
  Windows ADK WinPE image.
---

# Build a Boot Image

Use `Build-OSDeployBoot` to customize a Windows Preinstallation Environment (WinPE) image, create bootable media and ISO files, and record the build configuration and logs under OSDeploy Core.

<figure><img src="../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
OSDeploy creates and maintains the boot media on the workstation. OSDCloud runs from that media on the target device and performs the Windows deployment.
{% endhint %}

## Build the Boot Media

{% stepper %}
{% step %}
### Confirm the Requirements

Run the function on a workstation that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../../osdeploy-guide/requirements/powershell-modules.md)
* Configured [OSDeploy Core](update-osdeploycore.md)
* [Windows ADK and WinPE add-on](/broken/pages/KKnKou096GC0HYAS6jiH)
* OSDCloud module version `26.7.25.2` or later
* Administrator rights
* `curl.exe` available in `PATH`

{% hint style="warning" %}
The function stops before source selection when Windows, PowerShell, `curl.exe`, administrator access, OSDCloud, or Windows ADK requirements are not met. The Windows ADK is required even when the build source is an imported WinRE image.
{% endhint %}
{% endstep %}

{% step %}
### Run the Build

Open an elevated PowerShell 7.6 session and provide a name for the build:

```powershell
Build-OSDeployBoot -Name 'MyPE'
```

Select an imported WinRE image, a saved profile or shared content, and a wallpaper when prompted. The default workflow uses these settings:

| Setting         | Default                                                       |
| --------------- | ------------------------------------------------------------- |
| Source image    | Selected WinRE image, with Windows ADK WinPE fallback         |
| Architecture    | Selected WinRE architecture, or host architecture on fallback |
| Timezone        | Current system timezone                                       |
| ADK packages    | Installed                                                     |
| USB update      | Disabled                                                      |
| Existing folder | Add a numeric suffix instead of overwriting                   |

The function validates the environment, loads or creates a build profile, prepares and mounts `boot.wim`, adds the selected packages and content, saves the image, and creates the bootable media and ISO files. When compatible Secure Boot files exist in the selected WinRE source, it also creates CA 2023 media. ADK-sourced builds do not create the additional CA 2023 media.
{% endstep %}
{% endstepper %}

For source selection, profiles, customization, `WhatIf`, and advanced examples, see [Build-OSDeployBoot](../cmdlets/build-osdeployboot.md). For compact syntax and parameter definitions, see the [Build-OSDeployBoot command reference](../../command-reference/osdeploy/build-osdeployboot.md).

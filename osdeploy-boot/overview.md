---
description: Understand how Build-OSDeployBoot creates customized WinPE boot media from a WinRE or Windows ADK source.
---

# Overview

OSDeploy Boot is the boot-media creation workflow provided by the `Build-OSDeployBoot` function. It assembles a customized Windows Preinstallation Environment (WinPE) image for OS deployment from either an imported Windows Recovery Environment (WinRE) image or the Windows ADK WinPE image.

The build runs on the OSDeploy Core Workstation. It uses content maintained in OSDeploy Core, applies the selected configuration to `boot.wim`, and produces bootable media that can be used to start a target device.

{% hint style="info" %}
OSDeploy creates and maintains the boot media on the workstation. OSDCloud runs from that media on the target device and performs the Windows deployment.
{% endhint %}

## Build Process

`Build-OSDeployBoot` performs the following major steps:

1. **Validate the build environment.** The function verifies Windows, PowerShell, administrator access, the OSDCloud module, and the Windows ADK before making changes.
2. **Select the source image.** The build uses either an imported WinRE image or the Windows ADK WinPE image for the selected AMD64 or ARM64 architecture.
3. **Load the build configuration.** OSDeploy loads or creates a build profile containing the selected drivers, scripts, startup profiles, languages, regional settings, and wallpaper.
4. **Create the media workspace.** The function creates the build directories, copies the Windows ADK media files, prepares `boot.wim`, and mounts the image for servicing.
5. **Customize the image.** The build adds ADK optional components, PowerShell updates, OSDeploy and OSDCloud modules, applications, drivers, scripts, startup settings, console settings, environment variables, and branding.
6. **Finalize the image.** OSDeploy saves and dismounts the serviced image, exports build information, and creates the bootable media and ISO output.
7. **Write supporting output.** The function records the build profile, build context, image properties, package information, and logs. When requested, it also copies the completed media to a USB partition labeled `USB-WinPE`.

## Completed BootMedia

Completed builds are written under:

```powershell
$env:ProgramData\OSDeployCore\boot
```

Each build has its own folder named from the Windows build version, architecture, and name supplied to `Build-OSDeployBoot`. For example:

```text
C:\ProgramData\OSDeployCore\boot\26200.1234-amd64-MyPE
```

The completed BootMedia file structure is located in the `bootmedia` subfolder:

```text
C:\ProgramData\OSDeployCore\boot\26200.1234-amd64-MyPE\bootmedia
```

The serviced WinPE image is located at `bootmedia\sources\boot.wim`. The build folder also contains metadata and logs that describe how the media was created. If a folder with the same build name already exists, OSDeploy appends a numeric suffix such as `-001` rather than overwriting it.

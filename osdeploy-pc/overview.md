# Overview

OSDeploy provides IT administrators with a repeatable way to create and maintain OSDCloud boot images. It builds on the Windows Assessment and Deployment Kit (ADK), combining the ADK deployment tools and optional components with the PowerShell modules, drivers, applications, scripts, and settings required by OSDCloud.

Without OSDeploy, an administrator must assemble and service these components manually with DISM and other ADK tools. OSDeploy organizes the source files under `$env:ProgramData\OSDeployCore`, applies the selected customizations, and produces consistent boot media under `$env:ProgramData\OSDeployCore\boot`.

OSDeploy creates the boot image on the OSDeploy PC. OSDCloud runs from that image on the target device and performs the Windows deployment.

{% hint style="info" %}
The OSDeploy PC is the PC where you run OSDeploy for boot-image creation and maintenance. Do not use these instructions as a guide for running OSDCloud on a target device.
{% endhint %}

## Why Use OSDeploy?

OSDeploy turns boot-image creation into a controlled administrative workflow. Use it to:

* Build from an imported Windows Recovery Environment (WinRE) image or the Windows ADK WinPE image.
* Add the ADK optional components required for PowerShell and deployment tasks.
* Add current OSDeploy, OSDCloud, and optional OSD PowerShell modules.
* Add WinPE drivers, utilities, scripts, startup profiles, language settings, and branding.
* Generate repeatable boot media, ISO files, and USB media from a maintained local source.
* Retain build output and logs so an administrator can review and reproduce an image.

This gives IT administrators a stable method for updating boot images as Windows, drivers, and deployment tooling change.

## Wireless Support

The image source determines whether wireless networking can be supported:

| Image source              | Wireless support                                                                                                                     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Windows ADK WinPE         | Wireless hardware is not supported. OSDeploy excludes Wi-Fi drivers from these builds.                                               |
| Imported Windows 11 WinRE | Wireless networking can be supported when the image contains the required Windows components and compatible Wi-Fi drivers are added. |

Use a WinRE-based OSDCloud boot image when target devices must connect without wired Ethernet. OSDeploy can maintain Microsoft and vendor wireless drivers in OSDeploy Core and add selected drivers during the build.

{% hint style="warning" %}
Adding Wi-Fi drivers to an ADK WinPE image does not enable wireless networking. Build from an imported WinRE source when wireless support is required.
{% endhint %}

## Use a Dedicated Build Environment

Use a standalone PC or a dedicated virtual machine as the OSDeploy PC. A dedicated environment keeps the Windows ADK, PowerShell, modules, drivers, cached operating systems, and build configuration isolated from normal administrative work.

This reduces unexpected changes between builds and makes the resulting boot images easier to reproduce. Keep the OSDeploy PC updated, but control changes to its deployment toolchain.

## OSDeploy PC Baseline

Configure the OSDeploy PC with the following baseline:

| Requirement        | Baseline                                                                                                         |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| Operating system   | Windows 11 25H2 amd64                                                                                            |
| PowerShell         | Latest PowerShell 7 installed from the MSI package                                                               |
| Permissions        | Local administrative rights                                                                                      |
| Storage            | At least 50 GB of free space on the system volume                                                                |
| Network            | Internet access to Microsoft, GitHub, PowerShell Gallery, Recast Software, and hardware-vendor download services |
| PowerShell modules | OSDeploy and OSDCloud; OSD is optional                                                                           |
| Registration       | Recast Software Community License (optional)                                                                     |
| Environment        | Standalone PC or dedicated virtual machine                                                                       |

Windows 11 on arm64 can potentially be used, but this configuration is not fully tested. Other Windows client versions and Windows Server are unsupported.

## Setup Sequence

Complete the OSDeploy PC setup in this order:

1. Review the [system requirements](windows-11-os.md) and install current Windows updates.
2. Complete the required [Windows 11 configuration](windows-11-optional.md).
3. Install the latest [PowerShell 7 MSI package](powershell-7.md).
4. Install the required [PowerShell modules](powershell-modules.md).
5. Optionally obtain and install the [Recast Software Community License](community-registration.md).

After completing these steps, use the OSDeploy module to populate OSDeploy Core and create OSDCloud boot images.

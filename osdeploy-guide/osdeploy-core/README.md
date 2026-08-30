---
description: >-
  Understand how OSDeploy Core provides the local content used to create and
  maintain OSDeploy boot images.
---

# OSDeploy Core

OSDeploy Core is the local content library that OSDeploy uses to create boot images. It keeps the source files, supporting tools, drivers, and metadata required by the build process under `$env:ProgramData\OSDeployCore`.

The OSDeploy PowerShell module provides the commands that prepare and use this content. OSDeploy Core provides the maintained source library those commands work from. Separating the content from the installed module allows OSDeploy to update the module without discarding downloaded operating systems, expanded drivers, cached applications, or previous build output.

## Role in Boot-Image Creation

A boot image is assembled from several independent sources. Depending on the selected workflow, OSDeploy may need Windows ADK files, an imported Windows Recovery Environment (WinRE), WinPE optional components, network and storage drivers, PowerShell modules, and utilities that must run in WinPE.

OSDeploy Core organizes these sources before the build starts. `Build-OSDeployBoot` can then select the appropriate content, apply it to the chosen WinPE or WinRE source, and write the completed boot image and related media back to the OSDeploy Core workspace.

{% hint style="info" %}
OSDeploy creates and maintains the boot image on the OSDeploy PC. OSDCloud runs from that image on the target device and performs the Windows deployment.
{% endhint %}

## Core Content

OSDeploy Core can contain:

* Windows 11 ESD files and imported operating system media.
* Windows RE images used as boot-image sources.
* Windows ADK and WinPE supporting files.
* Vendor and Microsoft WinPE drivers.
* Utilities and application files added to WinPE.
* Catalogs, metadata, logs, and completed boot-media output.

Content is cached by version and architecture where required so AMD64 and ARM64 sources can be maintained independently.

## Why It Is Necessary

The Windows ADK supplies the basic tools for servicing Windows images, but it does not provide a complete OSDCloud boot image. The image still needs a supported Windows source, deployment components, current drivers, PowerShell content, and any required utilities or customizations.

OSDeploy Core gives OSDeploy a predictable location for that material. This makes builds easier to maintain and reproduce, reduces repeated downloads, and preserves prepared content between module updates. It also keeps source content and build output separate from the module installation itself.

Use the guides in this section to [install required software](../software/), [prepare Windows 11 content](update-osdeploycore/update-windows-11-os/), and [maintain WinPE drivers](update-osdeploycore/update-winpe-drivers/) before creating a boot image.

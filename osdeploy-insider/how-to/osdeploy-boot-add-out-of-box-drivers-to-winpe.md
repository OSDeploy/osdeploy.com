---
description: Add extracted third-party WinPE drivers to the shared OSDeploy Boot-Assets library.
---

# OSDeploy Boot: Add Out-of-Box Drivers to WinPE

Add storage, network, or platform drivers when the built-in OSDeploy driver catalog does not cover the target hardware. OSDeploy adds drivers recursively from architecture-specific folders while `Build-OSDeployBoot` services the mounted image.

{% hint style="warning" %}
Use drivers intended for Windows PE and the architecture of the boot image. Do not mix `amd64` and `arm64` drivers in the same folder. Test custom drivers on representative hardware before distributing the image.
{% endhint %}

## Prepare the Driver Files

Extract the vendor package until the driver files are visible. The folder must contain at least one `.inf` file; do not copy only an `.exe`, `.msi`, `.cab`, or `.zip` installer.

Confirm the extracted package contains INF files:

```powershell
$Source = 'C:\Drivers\Contoso-WinPE'

Get-ChildItem -Path $Source -Filter '*.inf' -File -Recurse |
    Select-Object FullName
```

If this command returns nothing, extract the package again or obtain the vendor's raw WinPE driver pack.

## Add Drivers to the Shared Library

Choose the root that matches the boot-image architecture:

```text
C:\ProgramData\OSDeployCore\boot-assets\winpedrivers-amd64
C:\ProgramData\OSDeployCore\boot-assets\winpedrivers-arm64
```

Copy an extracted AMD64 package into its own descriptive subfolder:

```powershell
$Source = 'C:\Drivers\Contoso-WinPE'
$Destination = Join-Path $env:ProgramData `
    'OSDeployCore\boot-assets\winpedrivers-amd64\Contoso-Model'

New-Item -Path $Destination -ItemType Directory -Force | Out-Null
Copy-Item -Path (Join-Path $Source '*') -Destination $Destination -Recurse -Force
```

Verify that OSDeploy can discover the INF files:

```powershell
Get-ChildItem -Path $Destination -Filter '*.inf' -File -Recurse |
    Select-Object FullName
```

## Select and Build

Start an interactive build:

```powershell
Build-OSDeployBoot
```

Select the new folder when the WinPE driver picker appears. OSDeploy displays only the shared driver root that matches the selected boot-image architecture and adds the selected folder recursively. ADK-sourced WinPE builds exclude Wi-Fi selections because that source does not support wireless networking.

## Verify the Build

Review the build output for driver-addition errors and confirm that `boot.wim` was created. Boot the ISO on representative hardware and verify the required device:

```powershell
Get-PnpDevice |
    Where-Object Status -ne 'OK' |
    Format-Table Status, Class, FriendlyName, InstanceId
```

See [OSDeployCore Boot-Assets](../reference/osdeploycore-boot-assets.md) for discovery and precedence details, and [OSDeploy Core: Removing Unused WinPE Drivers](osdeploy-core-removing-unused-winpe-drivers.md) when old versions are no longer needed.

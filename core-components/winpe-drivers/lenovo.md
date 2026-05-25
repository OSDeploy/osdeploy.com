# Lenovo WinPE Drivers

Lenovo provides WinPE driver packs for injecting Lenovo hardware drivers into WinPE boot images, enabling network and storage connectivity during OS deployment on Lenovo devices.

| Property | Value |
|---|---|
| Publisher | Lenovo |
| Architecture | amd64 |
| Platform | WinPE |
| Source | [Lenovo WinPE Driver Packs](https://support.lenovo.com/solutions/ht074984) |

{% embed url="https://support.lenovo.com/solutions/ht074984" %}
{% endembed %}

## Overview

Lenovo publishes WinPE driver packs for ThinkPad, ThinkCentre, ThinkStation, and IdeaPad device families. Driver packs are distributed as self-extracting executables and target the WinPE environment for network and storage enablement.

{% hint style="info" %}
Lenovo WinPE drivers are not automatically downloaded by `Update-OSDeployCoreDrivers`. Download the pack manually from the Lenovo support site and extract the drivers to `C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers`. Once placed there, `Build-OSDeployBoot` will inject them automatically and `Get-OSDeployCoreDrivers` will include them in its output.
{% endhint %}

---

## Manual Download

Visit the Lenovo WinPE Driver Pack support page and download the pack for your target device family:

[https://support.lenovo.com/solutions/ht074984](https://support.lenovo.com/solutions/ht074984)

Extract the downloaded package directly into the OSDRepo driver library:

```powershell
$DestinationPath = 'C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\lenovo'
New-Item -Path $DestinationPath -ItemType Directory -Force | Out-Null
# Run the downloaded self-extractor with the extraction path argument
& "$env:USERPROFILE\Downloads\LenovoWinPEDriverPack.exe" /VERYSILENT /DIR=$DestinationPath
```

{% hint style="info" %}
The extraction command varies by package. Check the Lenovo support page for the exact switches for the pack you downloaded.
{% endhint %}

Verify the drivers are visible to OSDeploy:

```powershell
Get-OSDeployCoreDrivers -Architecture amd64
```

---

## Related

- [Lenovo WinPE Driver Packs](https://support.lenovo.com/solutions/ht074984)
- [WinPE Drivers overview](README.md)
- [Get-OSDeployCoreDrivers](https://docs.osdeploy.com)

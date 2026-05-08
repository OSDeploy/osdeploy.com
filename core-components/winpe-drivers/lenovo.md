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
Lenovo WinPE drivers are available for manual download from the Lenovo support site. Automated download and injection via `Update-OSDeployWinPEDrivers` is not yet available for Lenovo. Download the pack manually and place the extracted drivers in a folder that `Build-OSDeployBootMedia` can reference.
{% endhint %}

---

## Manual Download

Visit the Lenovo WinPE Driver Pack support page and download the pack for your target device family:

[https://support.lenovo.com/solutions/ht074984](https://support.lenovo.com/solutions/ht074984)

Extract the downloaded package to your driver staging folder:

```powershell
$DestinationPath = 'C:\WinPEDrivers\Lenovo'
New-Item -Path $DestinationPath -ItemType Directory -Force | Out-Null
# Run the downloaded self-extractor with the extraction path argument
& "$env:USERPROFILE\Downloads\LenovoWinPEDriverPack.exe" /VERYSILENT /DIR=$DestinationPath
```

{% hint style="info" %}
The extraction command varies by package. Check the Lenovo support page for the exact switches for the pack you downloaded.
{% endhint %}

---

## Related

- [Lenovo WinPE Driver Packs](https://support.lenovo.com/solutions/ht074984)
- [WinPE Drivers overview](README.md)
- [Get-OSDeployWinPEDrivers](https://docs.osdeploy.com)

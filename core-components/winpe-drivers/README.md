# WinPE Drivers

WinPE driver packs are vendor-provided archives of hardware drivers that OSDeploy injects into WinPE boot images to enable network and storage connectivity during OS deployment.

| Property | Value |
|---|---|
| Architecture | amd64 (all vendor packs) |
| Platform | WinPE |
| Required by | `Build-OSDeployBootMedia`, `Update-OSDeployBootMedia` |
| Managed by | `Update-OSDeployWinPEDrivers` |

## Overview

WinPE does not include NIC, WiFi, or storage drivers by default. Without drivers, WinPE cannot connect to the network to download Windows images, OEM driver packs, or run OSDCloud. OSDeploy automates the download and injection of vendor driver packs from Dell, HP, Lenovo, Intel, and Microsoft. Drivers are downloaded and expanded into the OSDeployCore library, then injected into the boot image at build time.

All vendor-provided WinPE driver packs target the **amd64** architecture. WiFi drivers are only needed for WinRE-based boot images — ADK-generated WinPE does not include wireless hardware support by default.

## Why OSDeploy Requires WinPE Drivers

- WinPE boots without NIC or storage drivers on most modern hardware without vendor packs
- `Update-OSDeployWinPEDrivers` downloads and expands driver packs into the OSDeployCore repository
- `Build-OSDeployBootMedia` injects drivers from the repository into the WinPE image at build time
- `Get-OSDeployWinPEDrivers` returns the set of driver folders currently in the library

{% hint style="info" %}
WiFi drivers are excluded automatically when no imported OS sources are present. ADK WinPE does not support wireless hardware — WiFi drivers are only needed for WinRE-based boot images.
{% endhint %}

## In This Section

- [Dell](dell.md) — Dell WinPE 11 driver pack (amd64)
- [HP](hp.md) — HP WinPE driver pack (amd64)
- [Lenovo](lenovo.md) — Lenovo WinPE driver pack (amd64)
- [Intel Ethernet](intel-ethernet.md) — Intel Ethernet adapter complete driver pack (amd64)
- [Intel Wireless](intel-wireless.md) — Intel ProSet Wireless IT administrator driver pack (amd64)
- [Microsoft Features](microsoft-features.md) — Microsoft Windows Client LOF Packages ISO: Ethernet and WiFi drivers (amd64)

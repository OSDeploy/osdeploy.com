# Intel Ethernet WinPE Drivers

Intel provides a complete Ethernet adapter driver pack for injecting Intel NIC drivers into WinPE boot images, enabling network connectivity on systems with Intel Ethernet controllers.

| Property | Value |
|---|---|
| Publisher | Intel Corporation |
| Architecture | amd64 |
| Platform | WinPE |
| Source | [Intel Ethernet Adapter Complete Driver Pack](https://www.intel.com/content/www/us/en/download/15084/intel-ethernet-adapter-complete-driver-pack.html) |
| Expand command | `Expand-Archive` (ZIP archive) |

{% embed url="https://www.intel.com/content/www/us/en/download/15084/intel-ethernet-adapter-complete-driver-pack.html" %}
{% endembed %}

## Overview

The Intel Ethernet Adapter Complete Driver Pack is a ZIP archive containing `.inf`-based drivers for all supported Intel Ethernet controllers. OSDeploy downloads the pack, extracts it using `Expand-Archive`, and injects the relevant drivers into the WinPE image at build time. This pack is most useful when deploying to systems with Intel NICs that are not covered by the Dell or HP vendor packs.

## Download with OSDeploy

Use `Update-OSDeployWinPEDrivers` to refresh the Intel Ethernet catalog and download the driver pack.

**Preview and select packages interactively (Out-GridView picker):**

```powershell
Update-OSDeployWinPEDrivers -Name 'intel-ethernet'
```

**Download all Intel Ethernet packages without prompting:**

```powershell
Update-OSDeployWinPEDrivers -Name 'intel-ethernet' -NonInteractive
```

**Download only (skip expansion):**

```powershell
Update-OSDeployWinPEDrivers -Name 'intel-ethernet' -DownloadOnly
```

**View downloaded Intel Ethernet driver folders in the library:**

```powershell
Get-OSDeployWinPEDrivers -Architecture amd64
```

---

## Manual Download

Visit the Intel download page and download the latest complete driver pack ZIP:

[https://www.intel.com/content/www/us/en/download/15084/intel-ethernet-adapter-complete-driver-pack.html](https://www.intel.com/content/www/us/en/download/15084/intel-ethernet-adapter-complete-driver-pack.html)

Expand the ZIP to your driver staging folder:

```powershell
$DestinationPath = 'C:\WinPEDrivers\IntelEthernet'
Expand-Archive -Path "$env:USERPROFILE\Downloads\Intel-EthernetDriverPack.zip" -DestinationPath $DestinationPath
```

---

## Related

- [Intel Ethernet Adapter Complete Driver Pack](https://www.intel.com/content/www/us/en/download/15084/intel-ethernet-adapter-complete-driver-pack.html)
- [Update-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [Get-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [WinPE Drivers overview](README.md)

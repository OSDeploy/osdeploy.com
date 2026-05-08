# Intel Wireless WinPE Drivers

Intel provides an IT administrator wireless driver pack for injecting Intel Wi-Fi drivers into WinPE boot images, enabling wireless connectivity on systems with Intel wireless adapters.

| Property | Value |
|---|---|
| Publisher | Intel Corporation |
| Architecture | amd64 |
| Platform | WinPE (WinRE-based images) |
| Source | [Intel ProSet/Wireless Software and Wi-Fi Drivers for IT Administrators](https://www.intel.com/content/www/us/en/download/18231/intel-proset-wireless-software-and-wi-fi-drivers-for-it-administrators.html) |
| Expand command | `Expand-Archive` (ZIP archive) |

{% embed url="https://www.intel.com/content/www/us/en/download/18231/intel-proset-wireless-software-and-wi-fi-drivers-for-it-administrators.html" %}
{% endembed %}

## Overview

The Intel ProSet/Wireless IT administrator pack is a ZIP archive containing `.inf`-based drivers for Intel Wi-Fi 6, Wi-Fi 6E, and Wi-Fi 7 adapters. OSDeploy downloads the pack, extracts it using `Expand-Archive`, and injects drivers into the WinPE image at build time.

{% hint style="warning" %}
WiFi drivers are only needed for **WinRE-based boot images**. ADK-generated WinPE does not support wireless hardware. When no imported OS sources are present, `Update-OSDeployWinPEDrivers` excludes Wi-Fi packages automatically.
{% endhint %}

## Download with OSDeploy

Use `Update-OSDeployWinPEDrivers` to refresh the Intel Wireless catalog and download the driver pack. Pass `-SkipWifiDrivers` to exclude wireless packages when building a WinPE-only image.

**Preview and select packages interactively (Out-GridView picker):**

```powershell
Update-OSDeployWinPEDrivers -Name 'intel-wifi'
```

**Download all Intel Wireless packages without prompting:**

```powershell
Update-OSDeployWinPEDrivers -Name 'intel-wifi' -NonInteractive
```

**Download only (skip expansion):**

```powershell
Update-OSDeployWinPEDrivers -Name 'intel-wifi' -DownloadOnly
```

**View downloaded Intel Wireless driver folders in the library:**

```powershell
Get-OSDeployWinPEDrivers -Architecture amd64
```

**Exclude Wi-Fi drivers when building a WinPE-only image:**

```powershell
Update-OSDeployWinPEDrivers -SkipWifiDrivers
```

---

## Manual Download

Visit the Intel download page and download the latest IT administrator wireless driver pack ZIP:

[https://www.intel.com/content/www/us/en/download/18231/intel-proset-wireless-software-and-wi-fi-drivers-for-it-administrators.html](https://www.intel.com/content/www/us/en/download/18231/intel-proset-wireless-software-and-wi-fi-drivers-for-it-administrators.html)

Expand the ZIP to your driver staging folder:

```powershell
$DestinationPath = 'C:\WinPEDrivers\IntelWireless'
Expand-Archive -Path "$env:USERPROFILE\Downloads\Intel-WirelessDriverPack.zip" -DestinationPath $DestinationPath
```

---

## Related

- [Intel ProSet/Wireless Software for IT Administrators](https://www.intel.com/content/www/us/en/download/18231/intel-proset-wireless-software-and-wi-fi-drivers-for-it-administrators.html)
- [Update-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [Get-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [WinPE Drivers overview](README.md)

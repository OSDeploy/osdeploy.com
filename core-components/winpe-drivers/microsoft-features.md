# Microsoft Windows WinPE Drivers

Microsoft provides Ethernet and Wi-Fi drivers for WinPE through the Windows Client LOF (Language and Optional Features) Packages ISO, which contains OEM driver packages extracted from a full Windows 11 release.

| Property | Value |
|---|---|
| Publisher | Microsoft |
| Architecture | amd64 |
| Platform | WinPE |
| Package ID | `26100.1` |
| ISO filename | `26100.1.240331-1435.ge_release_amd64fre_CLIENT_LOF_PACKAGES_OEM.iso` |
| ISO size | ~5,962 MB |
| ISO download | [Microsoft software download (direct link)](https://software-static.download.prss.microsoft.com/dbazure/888969d5-f34g-4e03-ac9d-1f9786c66749/26100.1.240331-1435.ge_release_amd64fre_CLIENT_LOF_PACKAGES_OEM.iso) |
| Covers | `microsoft-windows-ethernet`, `microsoft-windows-wifi` |

## Overview

The Windows Client LOF Packages ISO contains OEM hardware packages from a Windows 11 26100.1 release build. OSDeploy mounts the ISO and extracts the Microsoft Ethernet and Wi-Fi driver packages into the WinPE driver library. These drivers are useful as a fallback when the Dell or HP vendor packs do not include sufficient NIC coverage for a specific device.

{% hint style="warning" %}
The LOF Packages ISO is approximately **6 GB**. Ensure sufficient disk space and a fast internet connection before initiating the download.
{% endhint %}

{% hint style="info" %}
WiFi drivers (`microsoft-windows-wifi`) are only needed for **WinRE-based boot images**. ADK-generated WinPE does not support wireless hardware. `Update-OSDeployWinPEDrivers` excludes Wi-Fi packages automatically when no imported OS sources are present.
{% endhint %}

## Download with OSDeploy

Use `Update-OSDeployWinPEDrivers` to download and expand Microsoft Ethernet and Wi-Fi drivers from the LOF Packages ISO.

**Preview and select packages interactively (Out-GridView picker):**

```powershell
Update-OSDeployWinPEDrivers -Name 'microsoft-windows-ethernet'
```

```powershell
Update-OSDeployWinPEDrivers -Name 'microsoft-windows-wifi'
```

**Download all Microsoft Windows driver packages without prompting:**

```powershell
Update-OSDeployWinPEDrivers -Name 'microsoft-windows-ethernet', 'microsoft-windows-wifi' -NonInteractive
```

**Exclude Wi-Fi when building a WinPE-only image:**

```powershell
Update-OSDeployWinPEDrivers -SkipWifiDrivers
```

**View downloaded Microsoft driver folders in the library:**

```powershell
Get-OSDeployWinPEDrivers -Architecture amd64
```

---

## Manual Download

Download the LOF Packages ISO directly from Microsoft:

```powershell
$IsoUrl = 'https://software-static.download.prss.microsoft.com/dbazure/888969d5-f34g-4e03-ac9d-1f9786c66749/26100.1.240331-1435.ge_release_amd64fre_CLIENT_LOF_PACKAGES_OEM.iso'
$IsoPath = "$env:ProgramData\OSDeployCore\cache\downloads\microsoft-windows-ethernet\26100.1.240331-1435.ge_release_amd64fre_CLIENT_LOF_PACKAGES_OEM.iso"

New-Item -Path (Split-Path $IsoPath) -ItemType Directory -Force | Out-Null
curl.exe --location --output $IsoPath --url $IsoUrl
```

{% hint style="info" %}
The ISO is approximately 6 GB. `curl.exe` is included with Windows 10 1803 and later and handles large downloads reliably.
{% endhint %}

---

## Related

- [Update-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [Get-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [WinPE Drivers overview](README.md)
- [Intel Ethernet WinPE Drivers](intel-ethernet.md)
- [Intel Wireless WinPE Drivers](intel-wireless.md)

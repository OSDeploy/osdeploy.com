# HP WinPE Drivers

HP provides a WinPE driver pack (SoftPaq) for injecting HP hardware drivers into WinPE boot images, enabling network and storage connectivity during OS deployment on HP devices.

| Property | Value |
|---|---|
| Publisher | HP Inc. |
| Architecture | amd64 |
| Platform | WinPE |
| Source | [HP WinPE Driver Pack](https://ftp.ext.hp.com/pub/caps-softpaq/cmit/HP_WinPE_DriverPack.html) |
| Expand command | `"{FileName}" /s /e /f "{DestinationPath}"` (HP SoftPaq self-extractor) |

{% embed url="https://ftp.ext.hp.com/pub/caps-softpaq/cmit/HP_WinPE_DriverPack.html" %}
{% endembed %}

## Overview

HP distributes its WinPE driver pack as a SoftPaq self-extracting executable. The pack covers HP EliteBook, ProBook, ZBook, EliteDesk, ProDesk, and Z workstation hardware. OSDeploy downloads the SoftPaq, extracts it using the HP self-extractor switches, and injects the drivers at boot image build time.

## Download with OSDeploy

Use `Update-OSDeployWinPEDrivers` to refresh the HP driver catalog and download the driver pack.

**Preview and select packages interactively (Out-GridView picker):**

```powershell
Update-OSDeployWinPEDrivers -Name 'hp'
```

**Download all HP packages without prompting:**

```powershell
Update-OSDeployWinPEDrivers -Name 'hp' -NonInteractive
```

**Download only (skip expansion):**

```powershell
Update-OSDeployWinPEDrivers -Name 'hp' -DownloadOnly
```

**View downloaded HP driver folders in the library:**

```powershell
Get-OSDeployWinPEDrivers -Architecture amd64
```

---

## Manual Download

Visit the HP WinPE Driver Pack page and download the latest SoftPaq:

[https://ftp.ext.hp.com/pub/caps-softpaq/cmit/HP_WinPE_DriverPack.html](https://ftp.ext.hp.com/pub/caps-softpaq/cmit/HP_WinPE_DriverPack.html)

Extract the SoftPaq to your driver staging folder:

```powershell
$DestinationPath = 'C:\WinPEDrivers\HP'
New-Item -Path $DestinationPath -ItemType Directory -Force | Out-Null
& "$env:USERPROFILE\Downloads\HPWinPEDriverPack.exe" /s /e /f $DestinationPath
```

---

## Related

- [HP WinPE Driver Pack](https://ftp.ext.hp.com/pub/caps-softpaq/cmit/HP_WinPE_DriverPack.html)
- [Update-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [Get-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [WinPE Drivers overview](README.md)

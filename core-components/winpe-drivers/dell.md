# Dell WinPE Drivers

Dell provides a WinPE 11 driver pack for injecting Dell hardware drivers into WinPE boot images, enabling network and storage connectivity during OS deployment on Dell devices.

| Property | Value |
|---|---|
| Publisher | Dell Technologies |
| Architecture | amd64 |
| Platform | WinPE |
| Source | [Dell WinPE 11 Driver Pack](https://www.dell.com/support/kbdoc/en-us/000211541/winpe-11-driver-pack) |
| Expand command | `expand.exe "{FileName}" -F:* "{DestinationPath}"` |

{% embed url="https://www.dell.com/support/kbdoc/en-us/000211541/winpe-11-driver-pack" %}
{% endembed %}

## Overview

The Dell WinPE 11 driver pack is a self-contained cabinet (`.cab`) archive that contains network, storage, and input drivers for the full range of Dell client and commercial devices. OSDeploy downloads the pack, expands it into the OSDeployCore repository using `expand.exe`, and injects the drivers at boot image build time.

## Download with OSDeploy

Use `Update-OSDeployWinPEDrivers` to refresh the Dell driver catalog and download the driver pack.

**Preview and select packages interactively (Out-GridView picker):**

```powershell
Update-OSDeployWinPEDrivers -Name 'dell'
```

**Download all Dell packages without prompting:**

```powershell
Update-OSDeployWinPEDrivers -Name 'dell' -NonInteractive
```

**Download only (skip expansion):**

```powershell
Update-OSDeployWinPEDrivers -Name 'dell' -DownloadOnly
```

**View downloaded Dell driver folders in the library:**

```powershell
Get-OSDeployWinPEDrivers -Architecture amd64
```

---

## Manual Download

Visit the Dell WinPE 11 Driver Pack support page and download the latest `.cab` file:

[https://www.dell.com/support/kbdoc/en-us/000211541/winpe-11-driver-pack](https://www.dell.com/support/kbdoc/en-us/000211541/winpe-11-driver-pack)

Expand the downloaded cab to your driver staging folder:

```powershell
$DestinationPath = 'C:\WinPEDrivers\Dell'
New-Item -Path $DestinationPath -ItemType Directory -Force | Out-Null
expand.exe "$env:USERPROFILE\Downloads\Dell-WinPE-Drivers.cab" -F:* $DestinationPath
```

---

## Related

- [Dell WinPE 11 Driver Pack](https://www.dell.com/support/kbdoc/en-us/000211541/winpe-11-driver-pack)
- [Update-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [Get-OSDeployWinPEDrivers](https://docs.osdeploy.com)
- [WinPE Drivers overview](README.md)

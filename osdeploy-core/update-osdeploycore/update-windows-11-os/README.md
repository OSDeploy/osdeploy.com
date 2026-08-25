---
description: >-
  Build Windows 11 setup media, Windows RE content, and WinPE driver sources
  from cached Enterprise ESD files.
---

# Update Windows 11 OS

{% hint style="info" %}
**TLDR:** Run `Update-OSDeployCoreOS` after downloading the Windows ESD files. Use `-Architecture` to import only one architecture.

```powershell
Update-OSDeployCoreOS
Update-OSDeployCoreOS -Architecture amd64
Update-OSDeployCoreOS -Architecture arm64
```
{% endhint %}

`Update-OSDeployCoreOS` transforms verified Windows 11 Enterprise ESD files into the Windows setup media, Windows RE cache, image metadata, and network driver sources used by OSDeploy Core.

The command does not download Windows. Run `Update-OSDeployCoreESD` first so the required ESD files are available in the local cache.

<figure><img src="../../../.gitbook/assets/image (355).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Run this command from an elevated PowerShell 7 session on Windows 11 25H2 build 26200. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.
{% endhint %}

## Select an Architecture

When `-Architecture` is omitted, the command imports every verified ESD returned by the OSDeploy Core cache. On an AMD64 workstation, this can include both AMD64 and ARM64 media. On an ARM64 workstation, the ESD download workflow provides ARM64 media.

Import only the AMD64 ESD:

```powershell
Update-OSDeployCoreOS -Architecture amd64
```

Import only the ARM64 ESD:

```powershell
Update-OSDeployCoreOS -Architecture arm64
```

The public parameter uses `amd64` and `arm64`. The command maps those values to the `_x64FRE_` and `_A64FRE_` segments in Microsoft ESD file names.

## Import Windows

Run the command without parameters to import all verified architectures in the cache:

```powershell
Update-OSDeployCoreOS
```

For each ESD, the command:

1. Reads the build and architecture from the ESD file name.
2. Expands the Windows Setup Media image.
3. Exports WinPE and Windows Setup into a two-index `boot.wim`.
4. Exports the Enterprise non-N image into `install.wim`.
5. Mounts `install.wim` read-only to collect Windows RE and supporting files.
6. Extracts Microsoft inbox Ethernet and Wi-Fi drivers for later WinPE use.
7. Writes image metadata and creates separate Windows OS and Windows RE caches.

The destination name combines the build, architecture, edition, and language:

```
26200.8653-amd64-enterprise-en-us
```

{% hint style="warning" %}
Image expansion, export, mounting, and content inspection can take several minutes and require substantial free space under `C:\ProgramData\OSDeployCore`.
{% endhint %}

## Review the Output

Windows setup media and its supporting files are written under:

```
C:\ProgramData\OSDeployCore\cache\windows-os\<destination-name>\
```

The primary directory contains:

| Path              | Content                                                               |
| ----------------- | --------------------------------------------------------------------- |
| `WinOS-Media\`    | Windows setup media with `sources\boot.wim` and `sources\install.wim` |
| `.wim\`           | Individual `winpe.wim`, `winse.wim`, and `winre.wim` files            |
| `.core\`          | Image metadata, boot files, and selected Windows files                |
| `.temp\`          | Registry hives, `ReAgent.xml`, and operation logs                     |
| `properties.json` | Windows OS identity and image properties                              |

The parallel Windows RE cache is written under:

```
C:\ProgramData\OSDeployCore\cache\windows-re\<destination-name>\
```

Microsoft inbox network drivers are staged by architecture under:

```
C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\
```

## Use Existing Imports

Before importing an ESD, the command checks for matching destination directories in both `windows-os` and `windows-re`.

If both directories exist, the import is complete and the command skips it. If only one directory exists, processing continues so an incomplete import can be rebuilt.

The command returns one `System.IO.DirectoryInfo` object for each Windows OS directory imported during the current run. A skipped existing import does not return another directory object.

```powershell
$WindowsOS = Update-OSDeployCoreOS -Architecture amd64
$WindowsOS | Select-Object Name, FullName
```

## Preview the Operation

Use `-WhatIf` to show the destination that would be imported without expanding or mounting the ESD:

```powershell
Update-OSDeployCoreOS -Architecture amd64 -WhatIf
```

`ShouldProcess` evaluates each destination separately. The command can still inspect the verified ESD cache and apply the architecture filter before the import is skipped.

## Review Detailed Output

Use `-Verbose` to display ESD selection, architecture filtering, destination naming, image index selection, driver metadata parsing, and DriverStore lookup details:

```powershell
Update-OSDeployCoreOS -Architecture amd64 -Verbose
```

DISM and file-copy logs are saved under the imported image's `.temp\logs` directory.

## Build Bootable Media

After the Windows OS and Windows RE directories exist, `Build-OSDeployBoot` can use the imported content to create bootable media:

```powershell
Build-OSDeployBoot
```

## Related

* [Update Windows 11 ESD](../update-windows-11-esd/)
* [Insider: Building an OS from an ESD](insider-export-windows-11.md)
* [Insider: Exporting Windows RE](insider-export-windows-re.md)
* [Insider: Exporting WinPE Drivers from an OS](export-winpe-drivers.md)
* [Update-OSDeployCoreOS command reference](../../../powershell-modules/osdeploy/Update-OSDeployCoreOS.md)
* [Build-OSDeployBoot command reference](../../../powershell-modules/osdeploy/Build-OSDeployBoot.md)

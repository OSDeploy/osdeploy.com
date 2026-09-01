---
description: >-
  Build Windows 11 setup media, Windows RE content, and WinPE driver sources
  from cached Enterprise ESD files.
---

# Update-OSDeployCoreOS

{% hint style="info" %}
`Update-OSDeployCoreOS` is the second public OSDeploy PowerShell module sub-function run by `Update-OSDeployCore`. It uses the ESD files from stage one to prepare Windows setup media and Windows Recovery Environment (WinRE) content. Run it independently when only this prepared content needs updating.

**TLDR:** Run `Update-OSDeployCoreOS` after downloading the Windows ESD files. Use `-Architecture` only when one architecture is needed.

```powershell
Update-OSDeployCoreOS
Update-OSDeployCoreOS -Architecture amd64
Update-OSDeployCoreOS -Architecture arm64
```
{% endhint %}

`Update-OSDeployCoreOS` transforms verified Windows 11 Enterprise ESD files into the Windows setup media, Windows RE cache, image metadata, and network driver sources used by OSDeploy Core.

The command does not download Windows. Run `Update-OSDeployCoreESD` first so the required ESD files are available in the local cache.

<figure><img src="../../.gitbook/assets/image (355).png" alt=""><figcaption></figcaption></figure>

## Requirements

Run this command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`. The DISM PowerShell cmdlets and `robocopy.exe` must be available, and the Core cache must contain at least one current-catalog Enterprise ESD with a matching SHA256 checksum.

{% hint style="warning" %}
This command does not download Windows media. If no verified current-catalog ESD is available after filtering, it writes a warning and returns without importing anything. Run `Update-OSDeployCoreESD` first.
{% endhint %}

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-Architecture` | `String` | Automatic | Use `amd64` or `arm64`. Omission processes every verified x64 and ARM64 ESD found for the newest bundled catalog. The filter matches `_x64FRE_` or `_A64FRE_` in the ESD file name. |
| `-WhatIf` | Common parameter | Not enabled | Reads and hashes current-catalog ESDs and resolves destinations, then skips each import at its destination-level `ShouldProcess` call. Core path initialization occurs before that gate. |
| `-Confirm` | Common parameter | Not enabled | Prompts separately before importing each destination that is not treated as a duplicate. |

## Select an Architecture

When `-Architecture` is omitted, the command imports every verified ESD returned by the OSDeploy Core cache. On an AMD64 OSDeploy PC, this can include both AMD64 and ARM64 media. On an ARM64 OSDeploy PC, the ESD download workflow provides ARM64 media.

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
C:\ProgramData\OSDeployCore\repository\build-winpedrivers\
```

## Use Existing Imports

Before importing an ESD, the command checks for matching destination directories in both `windows-os` and `windows-re`.

If both directories exist, the command treats the import as complete and skips it without validating their contents. If only one directory exists, processing continues in the existing paths so the missing side can be built.

The import is not transactional. It creates the Windows OS working directories before expanding media and locating the Enterprise non-N image. A DISM, mount, export, or copy failure can therefore leave partial content, and no rollback removes it. If no Enterprise non-N image is found, the command warns and leaves the work completed up to that point without returning a directory object.

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

`ShouldProcess` evaluates each destination separately after duplicate detection. The command can still initialize and migrate Core paths, inspect and hash the verified ESD cache, apply the architecture filter, and report existing imports before the import is skipped.

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

* [Complete OSDeploy Core update](update-osdeploycore.md)
* [Update Windows 11 ESD](update-osdeploycoreesd.md)
* [Insider: Building an OS from an ESD](../inside-osdeploy/insider-export-windows-11.md)
* [Insider: Exporting Windows RE](../inside-osdeploy/insider-export-windows-re.md)
* [Insider: Exporting WinPE Drivers from an OS](../inside-osdeploy/export-winpe-drivers.md)
* [Update-OSDeployCoreOS command reference](../../command-reference/osdeploy/update-osdeploycoreos.md)
* [Build-OSDeployBoot command reference](../../command-reference/osdeploy/build-osdeployboot.md)

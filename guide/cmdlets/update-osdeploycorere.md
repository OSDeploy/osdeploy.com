---
description: Export Windows Recovery Environment images and supporting Windows OS content from cached Enterprise ESD files.
---

# Update-OSDeployCoreRE

`Update-OSDeployCoreRE` is the second stage run by `Update-OSDeployCore`. It reads verified Enterprise ESD files from the current OSDeploy catalog, exports Windows Recovery Environment images, and stages the supporting Windows setup, image, boot, metadata, and network-driver content.

The command does not download Windows. Run `Update-OSDeployCoreESD` first so the required ESD files are available locally.

## Requirements

Run this command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe`, the DISM PowerShell cmdlets, and `robocopy.exe` must be available.

A valid Recast Software license is required when the command is called directly. If no valid license is found, the command displays license guidance and returns. The immediate call from `Invoke-OSDeployHydration` is the only license-gate exception.

The Core cache must contain at least one current-catalog Enterprise ESD with a matching SHA256 checksum.

{% hint style="warning" %}
This command does not download Windows media. If no verified current-catalog ESD remains after filtering, it writes a warning and returns. Run `Update-OSDeployCoreESD` first.
{% endhint %}

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-Architecture` | `String` | Automatic | Use `amd64` or `arm64`. Omission processes every verified x64 and ARM64 ESD found for the newest bundled catalog. |
| `-WhatIf` | Common parameter | Not enabled | Read and hash current-catalog ESDs and resolve destinations, then skip each export at its destination-level `ShouldProcess` call. Core initialization occurs before that gate. |
| `-Confirm` | Common parameter | Not enabled | Prompt separately before exporting each destination that is not already complete. |

## Examples

### Export every verified architecture

```powershell
Update-OSDeployCoreRE
```

### Export AMD64 content only

```powershell
Update-OSDeployCoreRE -Architecture 'amd64'
```

### Preview exports

```powershell
Update-OSDeployCoreRE -WhatIf
```

The preview can still initialize and migrate Core paths, verify ESD hashes, apply architecture filters, and report existing destinations.

## Export Process

For each verified ESD, the command:

1. Reads the build and architecture from the ESD filename.
2. Applies `ShouldProcess` to the destination.
3. Expands ESD index 1 as Windows Setup media.
4. Exports ESD indexes 2 and 3 as WinPE and Windows Setup images in `boot.wim`.
5. Selects the first Enterprise non-N image by image name and exports it as `install.wim`.
6. Mounts `install.wim` read-only to extract WinRE, boot files, registry hives, selected system files, and Ethernet and Wi-Fi drivers.
7. Writes metadata and creates the Windows OS and Windows RE cache layouts.

The selected install image is determined by image name, not a fixed ESD index.

## Created Content

Supporting Windows OS content is written under:

```text
%ProgramData%\OSDeployCore\cache\windows-os\<destination-name>\
```

The matching Windows RE content is written under:

```text
%ProgramData%\OSDeployCore\cache\windows-re\<destination-name>\
```

Microsoft inbox network drivers are staged by architecture under:

```text
%ProgramData%\OSDeployCore\winpedrivers-amd64\
%ProgramData%\OSDeployCore\winpedrivers-arm64\
```

The destination name combines the Windows build, architecture, edition, and language, such as `26200.8653-amd64-enterprise-en-us`.

Image servicing can take several minutes and requires substantial free space. The operation is not transactional: a DISM, mount, export, or copy failure can leave partial content for troubleshooting or cleanup.

## Existing Content

An ESD is treated as complete only when both its matching `windows-os` and `windows-re` destination directories exist. Complete exports are skipped without validating their contents and return no object. When only one destination exists, the command processes the ESD again to complete the pair.

## WhatIf and Confirmation

`ShouldProcess` evaluates each destination after ESD verification and duplicate detection. `-WhatIf` suppresses expansion, export, mount, and copy operations, but earlier Core initialization and cache inspection still occur. `-Confirm` prompts once for each destination that requires processing.

## Output

The command returns one `System.IO.DirectoryInfo` object for the supporting Windows OS destination created by each completed Windows RE export. Duplicate, declined, or skipped exports return no object.

```powershell
$WindowsOS = Update-OSDeployCoreRE -Architecture 'amd64'
$WindowsOS | Select-Object Name, FullName
```

Continue with [Build-OSDeployBoot](build-osdeployboot.md), or see the [Update-OSDeployCoreRE command reference](../../command-reference/osdeploy/update-osdeploycorere.md) for compact syntax.

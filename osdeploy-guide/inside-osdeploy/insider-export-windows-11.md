---
description: >-
  Follow how Update-OSDeployCoreOS turns a verified Enterprise ESD into Windows
  setup media and OSDeploy Core metadata.
---

# Insider: Export Windows 11

This article follows the main code path in `Update-OSDeployCoreOS` as it transforms a verified Enterprise ESD into a complete Windows setup-media directory.

The result is more than an exported `install.wim`. The function rebuilds `boot.wim`, records metadata for every image it uses, and collects selected content from a read-only Windows mount for later boot-image construction.

## Follow the Code Path

The OS import has five stages:

1. Resolve the cached ESD and derive its destination identity.
2. Expand the setup-media foundation.
3. export WinPE, Windows Setup, and Windows Enterprise images.
4. Build the final `boot.wim` and `install.wim` files.
5. Mount `install.wim` read-only to collect supporting content.

Each DISM operation writes a log under `.temp\logs`. The function returns the completed Windows OS directory only after all stages finish.

## Resolve the Source ESD

`Get-OSDeployCoreESD` supplies only ESD files whose SHA256 matches the bundled operating-system catalog. `Update-OSDeployCoreOS` can then filter those verified files by architecture:

```powershell
$esdFiles = Get-OSDeployCoreESD

if ($Architecture -and $esdFiles) {
	$archPattern = if ($Architecture -eq 'amd64') { '_x64FRE_' } else { '_A64FRE_' }
	$esdFiles = @($esdFiles | Where-Object { $_.Name -match $archPattern })
}
```

The command does not calculate the Windows identity from image metadata at this point. It first parses the build and architecture from the ESD file name:

| ESD file-name segment | OSDeploy value     |
| --------------------- | ------------------ |
| Leading `26200.8653`  | Build and revision |
| `_x64FRE_`            | `amd64`            |
| `_A64FRE_`            | `arm64`            |

These values form a destination ID such as:

```
26200.8653-amd64-enterprise-en-us
```

If the build or architecture cannot be parsed, the function warns and skips that ESD.

## Initialize the Import Directory

The Windows OS cache uses this root:

```
C:\ProgramData\OSDeployCore\cache\windows-os\
```

For each destination ID, the function creates four working areas:

| Directory      | Purpose                                                         |
| -------------- | --------------------------------------------------------------- |
| `.core\`       | Image metadata, boot files, and selected operating-system files |
| `.temp\`       | Temporary exports, registry hives, and operation logs           |
| `.wim\`        | Separate WinPE, Windows Setup, and Windows RE images            |
| `WinOS-Media\` | Bootable Windows setup-media layout                             |

It also writes `.core\id.json` so downstream functions can identify the directory without parsing its path.

If matching directories already exist in both the `windows-os` and `windows-re` caches, the function treats the import as complete and skips it. A partial import is not skipped when only one side exists.

## Understand the ESD Indexes

The first three ESD indexes have fixed setup roles. Windows editions begin at later indexes.

| ESD index   | Role                    | Destination                                |
| ----------- | ----------------------- | ------------------------------------------ |
| 1           | Windows Setup Media     | Expanded into `WinOS-Media\`               |
| 2           | Microsoft Windows PE    | `.wim\winpe.wim` and `boot.wim` index 1    |
| 3           | Microsoft Windows Setup | `.wim\winse.wim` and `boot.wim` index 2    |
| 4 and later | Windows editions        | Enterprise non-N exported to `install.wim` |

The first index is applied as a directory tree instead of exported as another WIM:

```powershell
Expand-WindowsImage `
	-ImagePath $esdPath `
	-Index 1 `
	-ApplyPath $DestinationMedia `
	-LogPath $CurrentLog
```

`Expand-WindowsImage` can mark expanded files as read-only. The function clears that attribute so OSDeploy can modify the media during later build steps.

## Build boot.wim

Indexes 2 and 3 are first exported into separate files:

```
.wim\winpe.wim
.wim\winse.wim
```

For each file, the function records:

* `Get-WindowsImage` output in JSON and CLIXML.
* `Get-WindowsImageContent` output in a text file.

The existing `WinOS-Media\sources\boot.wim` is then removed. The function exports `winpe.wim` into a new `boot.wim` and appends `winse.wim`:

| Final index | Image                   |
| ----------- | ----------------------- |
| 1           | Microsoft Windows PE    |
| 2           | Microsoft Windows Setup |

This restores the standard two-index boot image expected by Windows setup media.

## Export install.wim

The edition indexes are not assumed to be fixed. The function reads every ESD image and selects the first image whose name contains `Enterprise` but does not contain `Enterprise N`:

```powershell
$enterpriseEntry = Get-WindowsImage -ImagePath $esdPath |
	Where-Object {
		$_.ImageName -like '*Enterprise*' -and
		$_.ImageName -notlike '*Enterprise N*'
	} |
	Select-Object -First 1
```

That index is exported to:

```
WinOS-Media\sources\install.wim
```

If the ESD does not contain an Enterprise non-N image, the function warns and skips the remaining work for that ESD.

The exported image is always index 1 in the new `install.wim`. Its `Get-WindowsImage` properties become the authoritative architecture, language, version, edition, size, and content counts stored in the root `properties.json`.

## Record Image Metadata

The `.core` directory contains three representations for each WinPE, Windows Setup, Windows OS, and Windows RE image:

| File suffix                | Source                                      |
| -------------------------- | ------------------------------------------- |
| `-windowsimage.json`       | JSON serialization of `Get-WindowsImage`    |
| `-windowsimage.xml`        | CLIXML serialization of `Get-WindowsImage`  |
| `-windowsimagecontent.txt` | File listing from `Get-WindowsImageContent` |

The root `properties.json` is a smaller index for downstream OSDeploy commands. It includes the image ID, paths, architecture, languages, edition, version, image size, and file and directory counts.

## Mount the Operating System Read-Only

The final `install.wim` is mounted under a unique directory in `$env:TEMP`:

```powershell
$MountPath = Join-Path $env:TEMP "OSDeployCore-Mount-<unique-id>"
Mount-WindowsImage -ImagePath $DestinationImagePath -Index 1 -Path $MountPath -ReadOnly
```

The function uses that mount to collect content without servicing or committing changes to the WIM:

| Content                                                | Cached location                                     |
| ------------------------------------------------------ | --------------------------------------------------- |
| `SOFTWARE` and `SYSTEM` registry hives                 | `.temp\os-software.hive` and `.temp\os-system.hive` |
| `Windows\Boot`                                         | `.core\os-boot\`                                    |
| Selected System32 executables, DLLs, and related files | `.core\os-files\Windows\System32\`                  |
| Windows PowerShell modules                             | `.core\os-files\Program Files\WindowsPowerShell\`   |
| Windows RE image and metadata                          | `.wim\winre.wim` and `.core\winre-*`                |
| Inbox Ethernet and Wi-Fi drivers                       | `OSDRepo\winpe-drivers\`                            |

The selected operating-system files include tools and dependencies that can supplement a boot image, such as `curl.exe`, `makecab.exe`, `tar.exe`, `systeminfo.exe`, and Windows PowerShell modules. OSDeploy copies only the patterns defined by the function rather than the entire installed operating system.

## Preserve Logs and Clean Up

DISM logs use timestamped names for setup-media expansion and each WIM export. Robocopy writes separate logs for registry, boot-file, and OS-file operations.

The mount is enclosed in `try` and `finally`. The `finally` block always calls `Dismount-WindowsImage -Discard` and attempts to delete the temporary mount directory, including when extraction fails.

After dismounting, the function clears read-only attributes throughout the completed Windows OS directory. It then builds the separate Windows RE cache and returns the Windows OS `DirectoryInfo` object.

{% hint style="warning" %}
Do not remove the `.core`, `.temp`, or `.wim` directories because their names suggest disposable content. They contain metadata and source files consumed by later OSDeploy build operations.
{% endhint %}

## Inspect an Imported OS

List the imported image identity and primary WIM paths:

```powershell
$WindowsOSRoot = 'C:\ProgramData\OSDeployCore\cache\windows-os'
$ImportedOS = Get-ChildItem -Path $WindowsOSRoot -Directory |
	Sort-Object Name -Descending |
	Select-Object -First 1

Get-Content -Path (Join-Path $ImportedOS.FullName 'properties.json') -Raw |
	ConvertFrom-Json |
	Select-Object Id, Architecture, Version, EditionId, ImagePath
```

Inspect the indexes in the rebuilt boot image:

```powershell
$BootWim = Join-Path $ImportedOS.FullName 'WinOS-Media\sources\boot.wim'
Get-WindowsImage -ImagePath $BootWim |
	Select-Object ImageIndex, ImageName, Architecture, Version
```

## Related

* [Update Windows 11 OS](../cmdlets/update-osdeploycoreos.md)
* [Insider: The Windows ESD Catalog](insider-the-windows-esd-catalog.md)
* [Insider: Exporting Windows RE](insider-export-windows-re.md)
* [Insider: Exporting WinPE Drivers from an OS](export-winpe-drivers.md)
* [Update-OSDeployCoreOS command reference](../../command-reference/osdeploy/update-osdeploycoreos.md)

---
description: Technical reference for the folders, content, and creation workflows under the OSDeployCore cache.
---

# OSDeployCore Cache

OSDeploy stores reusable downloads, generated metadata, imported Windows images, and application packages under the following path:

```text
C:\ProgramData\OSDeployCore\cache
```

The OSDeploy module derives this path from `$env:ProgramData` when the module is imported. Most public workflows call the internal `Initialize-OSDeployCorePaths` function before they perform any work. This function creates the standard cache folders when they do not exist and does not remove existing content.

Commands that initialize the standard cache layout include `Build-OSDeployBoot`, `Install-OSDeploySoftware`, `Invoke-OSDeployHydration`, `Update-OSDeployCoreDrivers`, `Update-OSDeployCoreESD`, and `Update-OSDeployCoreRE`.

{% hint style="info" %}
The `cache` name does not mean that every folder is temporary. The `windows-os` and `windows-re` folders contain imported source images that must be imported again if deleted. The `config` and `osdeploymdt` folders contain locally generated configuration state.
{% endhint %}

## Folder summary

| Folder | Purpose | Created or populated by |
| --- | --- | --- |
| `config` | Locally generated OSDeploy configuration | Created by core path initialization; populated by WinPE driver catalog updates |
| `downloads` | Original WinPE driver package downloads | Created by core path initialization; populated by `Update-OSDeployCoreDrivers` |
| `hashes` | SHA256-keyed Windows image index metadata | Created by core path initialization; populated when OSDeploy inspects a WIM or ESD |
| `osdeploymdt` | MDT deployment share discovery and selection state | Created on demand by OSDeployMDT workflows |
| `psrepository` | PowerShell package cache used while servicing WinPE | Created by core path initialization; populated during boot image or MDT media servicing |
| `software` | Installers and offline layouts for host software | Created by core path initialization; populated by `Install-OSDeploySoftware` |
| `windows-os` | Imported Windows operating system and setup media | Created by core path initialization; populated by Windows source import workflows |
| `windows-re` | Imported Windows Recovery Environment images | Created by core path initialization; populated with the matching `windows-os` source |
| `winpe-apps` | Application binaries injected into WinPE | Created by core path initialization; populated during software installation or boot image servicing |

## `config`

The `config` folder stores machine-local configuration generated from current source data. It is separate from the module's built-in configuration so that module updates do not overwrite locally refreshed data.

The current file in this folder is:

```text
config\winpedrivers.json
```

`Update-OSDeployCoreDrivers` refreshes the WinPE driver catalog before downloading packages. The catalog workflow resolves current vendor metadata, merges refreshed sources with any existing entries, adds a `CatalogDate`, and writes the result to `winpedrivers.json`. Driver package discovery reads this file when it needs dynamic package metadata.

The folder is created by core path initialization. The driver catalog workflow also creates it directly if it is missing before writing `winpedrivers.json`.

## `downloads`

The `downloads` folder retains original WinPE driver package files. Packages are organized into source-specific subfolders, such as vendor or package identifiers, before they are verified and expanded into the managed WinPE driver library under `boot-assets`.

`Update-OSDeployCoreDrivers` populates this folder through the package-specific download handlers. Depending on the source, the retained file can be a CAB, EXE, ISO, MSI, or ZIP file. Downloads support reuse, and `-DownloadOnly` leaves the package in this folder without expanding it into the driver library.

The standard initializer creates the empty folder. Package handlers create any required source subfolders when a download is selected.

## `hashes`

The `hashes` folder avoids repeated DISM inspection of the same WIM or ESD file. OSDeploy calculates the SHA256 hash of the image and uses the hash as the JSON file name:

```text
hashes\<SHA256>.json
```

Each JSON file records the source path, file size, cache time, image count, and serializable metadata for every image index. When OSDeploy encounters the same image content again, it reads the JSON file instead of calling `Get-WindowsImage` for every index.

The standard initializer creates the folder. The internal `Get-WimIndexCache` function also creates it on demand and writes a new file when no matching hash exists. Deleting this folder does not delete source images; OSDeploy recalculates the hash and rebuilds the metadata during the next inspection.

## `osdeploymdt`

The `osdeploymdt` folder stores state used to bridge OSDeploy and Microsoft Deployment Toolkit. It contains:

```text
osdeploymdt\config.json
```

`Get-OSDeployMDTDeploymentShare` writes the deployment shares registered on the computer to the `DeploymentShares` property. `Select-OSDeployMDTDeploymentShare` adds the selected share as `MDTDriveName` and `MDTDrivePath`. Both functions preserve other properties already present in the file when they update their own values.

This folder is not part of the standard folder list created by `Initialize-OSDeployCorePaths`. It is created on demand when an OSDeployMDT workflow discovers or selects an MDT deployment share.

## `psrepository`

The `psrepository` folder is a local PowerShell package source used while OSDeploy services a WinPE image. It caches the configured PackageManagement, PowerShellGet, and NuGet packages needed to make PowerShell module installation work in WinPE.

During `Build-OSDeployBoot`, OSDeploy temporarily registers this folder as a trusted repository named `OSDeployCore`. Missing packages are downloaded from URLs defined in the module configuration. OSDeploy installs the required modules into the mounted image, copies the repository into the image as `X:\PSRepository`, and unregisters the temporary repository from the host session.

`Invoke-OSDeployMDT` uses the same cache when updating MDT boot media. Existing package files are reused; missing files are downloaded before servicing continues.

## `software`

The `software` folder contains host software installers and offline installation content downloaded by `Install-OSDeploySoftware`. Each component uses its own product and version folder. Examples include Windows ADK layouts, the Windows PE add-on, Microsoft Deployment Toolkit, and architecture-specific 7-Zip installers.

The standard initializer creates this folder. Each component installer creates its own subfolder and downloads content only as required by that component's implementation. `-DownloadOnly` populates supported component caches without installing the software on the host.

During path initialization, OSDeploy also migrates content from the legacy root-level path `C:\ProgramData\OSDeployCore\software` into `cache\software`. Existing destination conflicts are preserved and reported instead of being overwritten.

## `windows-os`

The `windows-os` folder contains complete imported Windows sources used by OSDeploy build workflows. Each source is stored in a directory named from its build, revision, architecture, edition, and language, for example:

```text
windows-os\26200.8457-amd64-enterprise-en-us
```

A source directory can contain:

| Item | Content |
| --- | --- |
| `.core` | Image, registry, boot, driver, and operating-system metadata |
| `.temp` | Working data and operation logs |
| `.wim` | Exported WinPE, Windows Setup, and Windows RE images |
| `WinOS-Media` | Windows Setup media files and the exported operating system image |
| `properties.json` | Source identity and Windows image metadata used for discovery |

The standard initializer creates the empty `windows-os` root. `Update-OSDeployCoreRE` populates it from verified Enterprise ESD files downloaded by `Update-OSDeployCoreESD`. The internal mounted-media import workflow can also populate it from a mounted Windows installation ISO. Existing destination names are not overwritten.

## `windows-re`

The `windows-re` folder contains the Windows Recovery Environment source paired with each imported `windows-os` source. Folder names match their corresponding operating system source so that OSDeploy can keep the two imports associated.

Each valid source contains `properties.json` and a WinRE image, normally at:

```text
windows-re\<source-name>\.wim\winre.wim
```

`Update-OSDeployCoreRE` creates the paired operating system and recovery sources from a verified ESD. The mounted-media import workflow performs the same pairing when it extracts `Windows\System32\Recovery\winre.wim` from an imported Windows image. `Build-OSDeployBoot` discovers these entries and selects a compatible source by architecture and version.

The standard initializer creates the empty folder, but only a successful source import creates usable child entries. If either a matching `windows-os` or `windows-re` destination already exists, the mounted-media import skips that source to avoid a partial or duplicate pair.

## `winpe-apps`

The `winpe-apps` folder caches portable application binaries that OSDeploy injects into mounted WinPE images. Current application-specific subfolders can include:

```text
winpe-apps\7zip
winpe-apps\microsoft-azcopy
winpe-apps\microsoft-powershell7
winpe-apps\microsoft-sysinternals
```

`Build-OSDeployBoot` creates or reuses these application caches as its servicing steps run. Missing archives or binaries are downloaded, and architecture-specific content is selected for the current build. The files are then copied or expanded into the mounted WinPE image.

`Install-OSDeploySoftware -Name '7zip'` also prepares the 7-Zip WinPE cache. This allows a later boot image build to reuse the files without downloading them again. Other application caches are populated on demand during boot image or MDT media servicing.

## Cache reuse and regeneration

OSDeploy generally treats an existing cache item as reusable and downloads or computes content only when the expected file or folder is absent. The exact reuse key depends on the folder:

| Content | Reuse key |
| --- | --- |
| Image metadata | SHA256 hash of the WIM or ESD |
| Driver download | Source folder and package file name |
| PowerShell package | Configured package file name |
| Software installer | Component, version, and architecture path |
| WinPE application | Application, version, architecture, or expected binary path |
| Windows source | Build-derived destination folder name |

Removing `downloads`, `hashes`, `psrepository`, `software`, or `winpe-apps` causes the relevant workflow to download or calculate the content again. Removing `config` discards the refreshed driver catalog. Removing `osdeploymdt` discards saved MDT discovery and selection state. Removing `windows-os` or `windows-re` removes imported source content and requires the source import workflow to run again.

For user-managed boot scripts, wallpapers, build profiles, startup profiles, and expanded WinPE drivers, see [OSDeployCore Boot-Assets](osdeploycore-boot-assets.md).

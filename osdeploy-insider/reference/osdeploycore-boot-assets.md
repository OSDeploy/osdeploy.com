---
description: Technical reference for shared and profile-local content under OSDeployCore Boot-Assets.
---

# OSDeployCore Boot-Assets

OSDeploy stores persistent boot image configuration and reusable build content under the following path:

```text
C:\ProgramData\OSDeployCore\boot-assets
```

`Initialize-OSDeployCorePaths` creates the standard Boot-Assets folders when they do not exist. Unlike `OSDeployCore\cache`, Boot-Assets is intended for administrator-managed content such as scripts, wallpapers, startup profiles, saved build profiles, and expanded WinPE drivers.

{% hint style="info" %}
Shared Boot-Assets content is normally selected while creating or updating a build profile. Content stored inside an individual profile directory is discovered automatically only when that profile is built. These are separate discovery paths.
{% endhint %}

## Folder summary

| Folder | Content | When it is used |
| --- | --- | --- |
| `boot-mediascript` | PowerShell scripts that modify completed boot media | After the WIM is dismounted and exported, before ISO creation and USB update |
| `boot-wallpaper` | JPG files available as WinPE backgrounds | When creating or updating a profile; the selected file is applied while the WIM is mounted |
| `boot-winpescript` | PowerShell scripts that customize the mounted WinPE image | During `Build-OSDeployBoot` while the WIM is mounted |
| `osdeployboot-profiles` | Saved build profiles and profile-local content | When selecting, creating, updating, previewing, or building a named profile |
| `winpedrivers-amd64` | Expanded amd64 WinPE driver packages | During amd64 boot image servicing |
| `winpedrivers-arm64` | Expanded arm64 WinPE driver packages | During arm64 boot image servicing |
| `winpestartup-profiles` | JSON configuration consumed by `Invoke-WinPEStartup` | Copied into the mounted image for use when WinPE starts |

## `boot-mediascript`

The `boot-mediascript` folder is the shared library for PowerShell scripts that operate on the boot media after image servicing is complete. Use these scripts for changes that require the final media directory rather than access to the mounted Windows image.

OSDeploy discovers `*.ps1` files at the root of `boot-mediascript` and one directory below it. The script selector also searches `core\boot-mediascript` in the installed OSDeploy, OSDCloud, and OSD modules when those modules are available. Files below deeper directory levels are not displayed by the selector.

Selected paths are stored in the build profile's `MediaScript` property. During `Build-OSDeployBoot`, OSDeploy runs the configured scripts in their saved order after the WIM has been dismounted and exported, but before the ISO is created or a USB drive is updated. Scripts execute in the current PowerShell session and can access `$global:BuildMedia`.

For a named build profile, OSDeploy also discovers scripts automatically from a profile-local `boot-mediascript` folder. Explicitly configured scripts run first, followed by profile-local scripts sorted by full path. Duplicate paths are removed case-insensitively.

## `boot-wallpaper`

The `boot-wallpaper` folder is the shared library for WinPE background images. Store JPG files directly in this folder; nested folders are not searched.

When a profile is created or updated, OSDeploy handles the available files as follows:

| JPG count | Behavior |
| --- | --- |
| None | No shared wallpaper is selected |
| One | The JPG is selected automatically |
| More than one | A single-selection picker is displayed |

The selected image is copied into the build profile directory as `wallpaper.jpg`. The profile therefore retains its own copy and does not depend on the original shared JPG during later builds.

While the WIM is mounted, `Build-OSDeployBoot` replaces `Windows\System32\winpe.jpg` with the profile-local `wallpaper.jpg`. If the profile does not contain that file, OSDeploy uses the module's bundled `core\boot-wallpaper\default.jpg`.

## `boot-winpescript`

The `boot-winpescript` folder is the shared library for PowerShell scripts that customize the WinPE image while it is mounted. These scripts can modify mounted-image files, registry hives, build variables, or other build state.

OSDeploy discovers `*.ps1` files at the folder root and one directory below it. It also searches `core\boot-winpescript` in the installed OSDeploy, OSDCloud, and OSD modules. When an architecture is known, filenames containing the opposite architecture name are excluded from the selection list.

Selected paths are stored in the profile's `WinPEScript` property. During `Build-OSDeployBoot`, they execute in the current PowerShell session near the end of mounted-image servicing, before profile-local WinPEStartup content and selected drivers are added.

A named profile can also contain its own `boot-winpescript` folder. Explicitly configured scripts run first, followed by profile-local scripts sorted by full path. Discovery is limited to the folder root and its immediate subdirectories, and duplicate paths run only once.

## `osdeployboot-profiles`

The `osdeployboot-profiles` folder contains persistent OSDeploy Boot build profiles. Each saved profile uses a canonical directory name that combines its name and architecture:

```text
osdeployboot-profiles\<Name>-<Architecture>\osdeployboot.json
```

For example:

```text
osdeployboot-profiles\BranchOffice-amd64\osdeployboot.json
```

The JSON file records the architecture, languages, regional settings, timezone, options, and explicitly selected paths for drivers, WinPE scripts, media scripts, and WinPEStartup profiles. OSDeploy converts supported absolute paths to portable tokens where possible:

| Token | Resolves to |
| --- | --- |
| `${{ OSDeployCore }}` | Current OSDeployCore data root |
| `${{ OSDeployModulePath }}` | Installed OSDeploy module root |
| `${{ OSDCloudModulePath }}` | Loaded OSDCloud module root |
| `${{ OSDModulePath }}` | Loaded OSD module root |

`New-OSDeployBootProfilePreview` creates a canonical profile after a Windows RE source and shared build content are selected. `Update-OSDeployBootProfilePreview` replaces the selected content and settings in an existing profile. `Build-OSDeployBoot -ProfileName` reads the saved JSON, expands path tokens, validates configured paths, and uses the profile without running the shared content selectors again.

Builds started without a named profile use a temporary profile-content directory. Their latest settings are also written as `recent-amd64.json` or `recent-arm64.json` at the root of `osdeployboot-profiles`; these recent files are not canonical named-profile directories.

### Profile-local content

A saved profile directory can carry content that travels with that profile:

```text
osdeployboot-profiles\BranchOffice-amd64\
|-- osdeployboot.json
|-- wallpaper.jpg
|-- boot-mediascript\
|   `-- Publish.ps1
|-- boot-winpescript\
|   `-- Configure-WinPE.ps1
|-- winpedrivers-amd64\
|   `-- Vendor\Device\driver.inf
`-- WinPEStartup\
	|-- profiles\
	|   `-- BranchOffice.json
	`-- assets\
		`-- Initialize-Network.ps1
```

This content does not need to be added to `osdeployboot.json`. When the named profile is built, OSDeploy derives these paths from the profile directory and processes them automatically.

| Profile-local content | Runtime behavior |
| --- | --- |
| `wallpaper.jpg` | Takes precedence over the module's bundled wallpaper |
| `boot-mediascript\*.ps1` | Runs after explicitly configured media scripts |
| `boot-winpescript\*.ps1` | Runs after explicitly configured WinPE scripts |
| `winpedrivers-<Architecture>` | Added recursively when the matching folder contains at least one INF file |
| `WinPEStartup\profiles\*.json` | Copied after explicitly selected startup profiles; a matching file name replaces the earlier copy |
| `WinPEStartup\assets` | Copied recursively into `WinPEStartup\assets` in the mounted image with overwrite enabled |

Profile initialization creates only `WinPEStartup\profiles` and `WinPEStartup\assets`. Add profile-local script and driver folders when the profile requires them.

## `winpedrivers-amd64`

The `winpedrivers-amd64` folder is the shared library for expanded amd64 WinPE driver packages. Each immediate child directory represents a selectable package. A package is displayed only when its directory contains at least one `*.inf` file somewhere below it.

`Update-OSDeployCoreDrivers -Architecture amd64` downloads source packages to the cache, verifies them when checksum data is available, and normally expands their driver content into this folder. Administrators can also add package directories manually.

When an amd64 build profile is created or updated, the driver selector displays matching package directories. Selected paths are stored in `WinPEDriver`. During `Build-OSDeployBoot`, each selected path is passed to `Add-WindowsDriver -Recurse -ForceUnsigned` while the WIM is mounted.

## `winpedrivers-arm64`

The `winpedrivers-arm64` folder has the same structure and behavior as `winpedrivers-amd64`, but contains packages for arm64 WinPE images.

`Update-OSDeployCoreDrivers -Architecture arm64` populates managed arm64 packages. The shared selector shows this folder only for arm64 profile selection, and the build injects selected paths recursively into an arm64 mounted image.

{% hint style="warning" %}
Keep driver architectures separated. Folder placement determines which architecture selector can offer the package; OSDeploy does not infer or correct the architecture from the INF files.
{% endhint %}

### Driver selection and Wi-Fi

The driver selector can exclude package names containing `wifi` or `wireless`. OSDeploy enables this behavior when the selected source is ADK WinPE because ADK WinPE does not provide the WinRE wireless support required by those drivers. WinRE-based builds can include compatible Wi-Fi packages.

Selected shared paths are processed before an architecture-matched profile-local driver folder. Duplicate paths are removed case-insensitively, and each remaining path is serviced recursively.

## `winpestartup-profiles`

The `winpestartup-profiles` folder is the shared library for JSON profiles consumed by `Invoke-WinPEStartup` after the built image starts. Profiles can control startup, main, shutdown, restart, module, Wi-Fi, IP configuration, device display, and PowerShell command behavior.

Store JSON files directly in this folder. The selector does not search subdirectories or filter profiles by architecture. It also includes root-level JSON files from `core\winpestartup-profiles` in the installed OSDeploy, OSDCloud, and OSD modules.

`New-WinPEStartupProfile` creates and validates a profile in this shared folder. During build profile creation or update, one or more startup profiles can be selected and their paths are stored in `WinPEStartupProfile`.

While the WIM is mounted, `Build-OSDeployBoot` copies the selected files into:

```text
WinPEStartup\profiles
```

Files from the selected build profile's local `WinPEStartup\profiles` folder are copied afterward. Because the copy uses overwrite behavior, a profile-local JSON file with the same file name replaces the explicitly selected file in the mounted image.

Profile-local `WinPEStartup\assets` is handled separately. It is not a shared top-level Boot-Assets folder and is not recorded in `osdeployboot.json`; all of its content is copied recursively into the mounted image whenever that profile is built.

## Folder creation

The standard folders are created by `Initialize-OSDeployCorePaths`, which is called by profile management and non-profile build workflows. Named profile builds use the existing selected profile and its content directly.

For downloaded packages, generated metadata, software installers, and imported Windows source images, see [OSDeployCore Cache](osdeploycore-cache.md).

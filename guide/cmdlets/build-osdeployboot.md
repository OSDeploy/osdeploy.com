---
description: >-
  Configure and build customized OSDeploy WinPE media from an imported WinRE
  image or the Windows ADK WinPE image.
---

# Build-OSDeployBoot

`Build-OSDeployBoot` services a WinRE or Windows ADK WinPE image and creates bootable media for OSDeploy and OSDCloud. Use command-line settings for a one-time build, or load an existing architecture-specific profile with `-ProfileName`.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

Install and configure these components before starting a build:

* [OSDeploy module](../requirements/powershell-modules.md)
* A valid Recast Software Community License for direct invocation
* [OSDeploy Core](../basic/update-osdeploycore.md)
* [Windows ADK and WinPE add-on](install-osdeploysoftware/windows-adk-25h2.md)
* OSDCloud module version `26.7.25.2` or later

Install or update OSDCloud with:

```powershell
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

{% hint style="warning" %}
The function stops before source selection when Windows, PowerShell, `curl.exe`, administrator access, licensing, OSDCloud, or Windows ADK requirements are not met. An invalid license displays `Show-OSDeployLicense`. The Windows ADK is required even when the build source is an imported WinRE image.
{% endhint %}

## Parameters

`Build-OSDeployBoot` has `Default` and `Profile` parameter sets for WinRE selection with ADK fallback, plus `ADK` and `ADKProfile` sets for explicitly selecting the ADK WinPE image.

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | None | Required in the `Profile` and `ADKProfile` sets. Specify the exact name of an existing architecture-specific profile, such as `Contoso-amd64`. Tab completion lists readable profiles. The command does not create or update the named profile. |
| `-Architecture` | `String` | Automatic | Use `amd64` or `arm64`. In WinRE sets, the value filters source selection. `-Auto` derives an omitted value from `PROCESSOR_ARCHITECTURE`. ADK builds require a value unless the profile supplies it. |
| `-Languages` | `String[]` | None | Add validated ADK languages. Use `*` to enumerate all available language directories except the independently processed `en-us` base. An explicit value overrides a loaded profile for this build only. |
| `-SetAllIntl` | `String` | Profile value or none | Pass a locale to the WinPE international-settings step. An explicit value overrides a loaded profile for this build only. |
| `-SetInputLocale` | `String` | Profile value or none | Pass an input locale to the WinPE servicing steps. An explicit value overrides a loaded profile for this build only. |
| `-SetTimeZone` | `String` | Profile value or current system timezone | Use a timezone returned by `tzutil /l`. The system default comes from `tzutil /g`. |
| `-SkipAdkPackages` | `Switch` | Not enabled | Skip ADK optional-component and language-package installation. Other image customization still runs. |
| `-UseAdkWinPE` | `Switch` | Not enabled | Required in the `ADK` and `ADKProfile` sets. Use the ADK `winpe.wim`. Cannot be combined with `-Auto`. |
| `-UpdateUSB` | `Switch` | Not enabled | After the build, run the USB update step against partitions labeled `USB-WinPE`. |
| `-Auto` | `Switch` | Not enabled | In the `Default` and `Profile` sets, select the newest compatible imported WinRE source without the source picker. Fall back to ADK WinPE when none is available. |
| `-Options` | `String[]` | Profile value or none | Use `pwsh`, `dart`, or both. An explicit value overrides a loaded profile for this build only. |
| `-WhatIf` | Common parameter | Not enabled | Complete prerequisite checks and configuration, then stop at the build-directory operation. Without `-ProfileName`, the recent profile snapshot can already have been written. |
| `-Confirm` | Common parameter | Not enabled | Prompt before creating the build output directories. Declining stops the build after configuration. |

Accepted `-Languages` values are:

```
*, ar-sa, bg-bg, cs-cz, da-dk, de-de, el-gr, en-gb, en-us, es-es,
es-mx, et-ee, fi-fi, fr-ca, fr-fr, he-il, hr-hr, hu-hu, it-it,
ja-jp, ko-kr, lt-lt, lv-lv, nb-no, nl-nl, pl-pl, pt-br, pt-pt,
ro-ro, ru-ru, sk-sk, sl-si, sr-latn-rs, sv-se, th-th, tr-tr,
uk-ua, zh-cn, zh-tw
```

## Examples

### Build interactively from WinRE

Select an imported WinRE image and shared content, write the architecture-specific recent profile, and build media named `OSDeploy`:

```powershell
Build-OSDeployBoot
```

If no WinRE image is selected or available, the function falls back to the Windows ADK WinPE image and derives the architecture from the host.

### Load an existing profile

Load `MyPE-amd64` without shared-content or wallpaper selectors, prompt for a matching WinRE source, and add PowerShell 7 for this build only:

```powershell
Build-OSDeployBoot -ProfileName 'MyPE-amd64' -Options 'pwsh'
```

The profile JSON and profile-local content are not changed.

### Build directly from ADK WinPE

Skip WinRE selection and use the AMD64 `winpe.wim` supplied by the Windows ADK:

```powershell
Build-OSDeployBoot `
	-Architecture 'amd64' `
	-UseAdkWinPE
```

ADK WinPE does not support wireless hardware. The function excludes selected driver paths whose leaf names contain `wifi` or `wireless`.

### Select the newest WinRE source automatically

Resolve the architecture from the host and select the newest imported WinRE image for that architecture:

```powershell
Build-OSDeployBoot -Auto
```

Without `-ProfileName`, shared driver, script, startup-profile, and wallpaper selection can still require interaction when matching content exists.

### Add languages and regional settings

Add French and Canadian French language packages, apply French regional settings, use a Canadian French keyboard, and set the timezone to Eastern Time:

```powershell
Build-OSDeployBoot `
	-Languages 'fr-fr', 'fr-ca' `
	-SetAllIntl 'fr-CA' `
	-SetInputLocale '0c0c:00001009' `
	-SetTimeZone 'Eastern Standard Time'
```

When a named profile is loaded, explicitly supplied language and international settings override the corresponding saved settings for this build only.

### Skip ADK package installation

Skip all ADK optional-component and language-package installation while retaining the remaining customization steps:

```powershell
Build-OSDeployBoot `
	-Architecture 'amd64' `
	-UseAdkWinPE `
	-SkipAdkPackages
```

Use this option only when the source image already contains the components required by the intended WinPE workflow.

### Update a prepared USB volume

Build the media and copy the standard `bootmedia` tree to each accessible volume labeled `USB-WinPE`:

```powershell
Build-OSDeployBoot -UpdateUSB
```

The update overwrites matching files but does not purge destination-only files. The function warns and continues when no matching volume is connected.

### Preview the build-directory operation

Resolve the source and profile configuration, display the build context, and stop before creating the build output tree:

```powershell
Build-OSDeployBoot -Auto -WhatIf
```

{% hint style="warning" %}
`-WhatIf` is not a read-only preview. Initialization and source/content selection occur before `ShouldProcess`. Without `-ProfileName`, the function writes the matching recent profile snapshot before stopping.
{% endhint %}

### Build ARM64 media with explicit settings

Build from the ARM64 ADK source, add German language packages, configure German regional settings, and update a prepared USB volume:

```powershell
Build-OSDeployBoot `
	-ProfileName 'ARM64-DE-arm64' `
	-Architecture 'arm64' `
	-UseAdkWinPE `
	-Languages 'de-de' `
	-SetAllIntl 'de-DE' `
	-SetInputLocale '0407:00000407' `
	-SetTimeZone 'W. Europe Standard Time' `
	-UpdateUSB
```

## Source Selection

The `Default` and `Profile` parameter sets start with an imported WinRE source:

1. With `-Auto`, derive an omitted architecture from `PROCESSOR_ARCHITECTURE`, find imported WinRE images for that architecture, sort by OS version descending, and select the newest image.
2. With `-Architecture`, show the interactive WinRE selector filtered to that architecture.
3. Without either option, show the interactive selector for all supported imported WinRE images.
4. If no WinRE image is selected or available, warn and fall back to ADK WinPE. When the architecture is still unset, derive it from the host.
5. Resolve the architecture-specific ADK paths. The function stops when the architecture or required ADK paths cannot be resolved.

The `ADK` and `ADKProfile` parameter sets bypass WinRE selection and use the architecture-specific ADK `winpe.wim`.

The build folder name uses this format:

```
{Windows build}.{revision}-{architecture}-{name}
```

The name is the selected profile name without its architecture suffix, or `OSDeploy` when no named profile is loaded. If that directory exists, the function tries `-001`, `-002`, and later suffixes until it finds an unused path.

## Build Profiles

Saved profiles use flat, architecture-qualified directories under:

```
%ProgramData%\OSDeployCore\repository\osdeployboot-profiles\{Name}-{Architecture}\osdeployboot.json
```

`-ProfileName` requires an exact readable profile. The saved profile supplies its architecture, explicit content paths, languages, international settings, timezone, and options. Explicit command-line configuration overrides saved values for the current build only. The function validates configured paths but does not display shared-content or wallpaper selectors and does not change the profile JSON or profile-local files.

| Token                       | Resolved module path           |
| --------------------------- | ------------------------------ |
| `${{ OSDeployModulePath }}` | Installed OSDeploy module base |
| `${{ OSDCloudModulePath }}` | Loaded OSDCloud module base    |
| `${{ OSDModulePath }}`      | Loaded OSD module base         |

When `-ProfileName` is omitted, the function presents shared selectors for compatible drivers, WinPE scripts, media scripts, WinPEStartup profiles, and wallpaper. Module-managed drivers are selected from `%ProgramData%\OSDeployCore\winpedrivers-{Architecture}`. User-managed drivers are selected from `%ProgramData%\OSDeployCore\repository\winpedrivers-{Architecture}`. It writes the configuration before build confirmation to:

```
%ProgramData%\OSDeployCore\repository\osdeployboot-profiles\recent-amd64.json
%ProgramData%\OSDeployCore\repository\osdeployboot-profiles\recent-arm64.json
```

The recent JSON snapshot remains after success, cancellation, a declined build operation, or a later terminating error. Selected wallpaper and other temporary companion content are removed after the build attempt. Shared scripts can come from the OSDeploy repository or the installed OSDeploy, OSDCloud, and OSD module roots. Use `New-OSDeployBootProfilePreview` and `Update-OSDeployBootProfilePreview` to manage named profiles.

## Profile-Local Content

Keep portable content beside `osdeployboot.json` to include it without storing additional absolute paths in the profile:

```
{profile}\
|-- osdeployboot.json
|-- build-mediascript\
|-- build-winpeapp\
|-- winpedrivers-{Architecture}\
|-- build-winpescript\
|-- build-winpewallpaper\
`-- WinPEStartup\
    |-- Profiles\
    `-- Scripts\
```

The build applies profile content as follows:

| Content               | Discovery and precedence                                                                                                                                                                                                                                                    |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WinPE app scripts     | Run explicit profile paths first, then sorted `*.ps1` files from `build-winpeapp` and its immediate subdirectories. Duplicate paths are ignored case-insensitively.                                                                                                         |
| WinPE scripts         | Run explicit profile paths first, then sorted `*.ps1` files from `build-winpescript` and its immediate subdirectories. Duplicate paths are ignored case-insensitively.                                                                                                      |
| Media scripts         | Run explicit profile paths first, then sorted `*.ps1` files from `build-mediascript` and its immediate subdirectories after the image is dismounted. Duplicate paths are ignored case-insensitively.                                                                        |
| WinPE drivers         | Add explicit paths first. When recursive `*.inf` files exist under the architecture-matched local `winpedrivers-amd64` or `winpedrivers-arm64` directory, append that directory once and let DISM add its drivers recursively.                                                            |
| WinPEStartup profiles | Copy explicit JSON files first, then sorted root-level JSON files from `WinPEStartup\Profiles`. A local file with the same destination name replaces the explicit file.                                                                                                     |
| WinPEStartup scripts  | Copy all content under `WinPEStartup\Scripts` recursively with overwrite. These paths are not stored in `osdeployboot.json`.                                                                                                                                                |
| Wallpaper             | Use the first JPG sorted by full path in `build-winpewallpaper`, then legacy `{profile}\wallpaper.jpg`, then the bundled default. When neither profile location contains a wallpaper, a shared wallpaper can be selected and saved as `build-winpewallpaper\wallpaper.jpg`. |

Missing configured content produces a warning and that item is skipped. A failing custom script writes a non-terminating error so later build steps can continue.

## Image Customization

Unless `-SkipAdkPackages` is used, the function installs the configured ADK optional components, their `en-us` resources, and each additional selected language. Use `*` to process every language directory available under the ADK optional-components path except `en-us`.

The function always attempts to set the selected timezone. It applies `Set-AllIntl` and `Set-InputLocale` only when their values are not empty. DISM output for these operations is written to timestamped files under `.temp\logs`.

The remaining build steps add OSDeploy components, OSDCloud, supported applications, Recast license files when present, drivers, custom scripts, startup content, console and environment settings, and wallpaper. The function then saves and exports `boot.wim`, runs media scripts, creates ISO files, and performs the optional USB update.

When compatible updated Secure Boot files exist in an imported WinRE source, the function also creates a `bootmedia_ca2023` tree and `bootmedia_ca2023.iso` for the CVE-2022-21894 mitigation. ADK-sourced builds do not create this additional media.

## WhatIf and Confirmation

The function calls `ShouldProcess` before creating the build output directories. Before that call, it performs requirement checks, initializes OSDeploy Core state, selects the source and content, loads a named profile or writes a recent snapshot, resolves wallpaper, populates `$global:BuildMedia`, displays the configuration, and waits five seconds.

With `-WhatIf`, the function stops at the directory operation. It does not create or service the build media, mount an image, create an ISO, or update USB media. A recent profile JSON and temporary companion content can be written before that gate. With `-Confirm`, declining the directory operation stops the command.

## Build Output

Successful builds are written under:

```
%ProgramData%\OSDeployCore\boot\{Windows build}.{revision}-{architecture}-{Name}
```

The stable output includes:

| Path                                     | Description                                                                     |
| ---------------------------------------- | ------------------------------------------------------------------------------- |
| `bootmedia\sources\boot.wim`             | Serviced WinPE image.                                                           |
| `bootmedia.iso`                          | Standard bootable ISO.                                                          |
| `bootmedia_ca2023\`                      | Updated Secure Boot media tree when compatible files exist in the WinRE source. |
| `bootmedia_ca2023.iso`                   | Updated Secure Boot ISO when the additional media tree is created.              |
| `.core\osdeployboot.json`                | Final build profile copied into the build metadata.                             |
| `.core\buildcontext.json`                | Serialized build context used by the build steps.                               |
| `.core\id.json`                          | Build identifier.                                                               |
| `.core\winpe-windowspackage-initial.xml` | Package inventory captured before package servicing.                            |
| `.temp\logs\`                            | Transcript, DISM, Robocopy, and build-step logs.                                |
| `properties.json`                        | Final image, source, configuration, content, ADK, and path metadata.            |

## Output

`Build-OSDeployBoot` does not write a result object to the PowerShell pipeline. Assigning the command to a variable produces no intentional output.

The function populates `$global:BuildMedia` for its build steps and leaves that process-wide state available for inspection. It contains paths, architecture, source type, profile settings, selected content, installed applications, and mounted-image state, but it is not a supported pipeline return object.

See [Build a Boot Image](../basic/build-osdeployboot.md) for the workflow overview or the [Build-OSDeployBoot command reference](../../command-reference/osdeploy/build-osdeployboot.md) for compact syntax and parameter definitions.

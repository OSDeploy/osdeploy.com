---
description: >-
  Configure and build customized OSDeploy WinPE media from an imported WinRE
  image or the Windows ADK WinPE image.
---

# Build-OSDeployBoot

`Build-OSDeployBoot` services a WinRE or Windows ADK WinPE image and creates bootable media for OSDeploy and OSDCloud. Use its parameters and build profiles to control the source architecture, ADK packages, languages, regional settings, drivers, applications, scripts, startup profiles, wallpaper, and optional USB update.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

Install and configure these components before starting a build:

* [OSDeploy module](../requirements/powershell-modules.md)
* [OSDeploy Core](../basic/update-osdeploycore.md)
* [Windows ADK and WinPE add-on](/broken/pages/KKnKou096GC0HYAS6jiH)
* OSDCloud module version `26.7.25.2` or later

Install or update OSDCloud with:

```powershell
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

{% hint style="warning" %}
The function stops before source selection when Windows, PowerShell, `curl.exe`, administrator access, OSDCloud, or Windows ADK requirements are not met. The Windows ADK is required even when the build source is an imported WinRE image.
{% endhint %}

## Parameters

`Build-OSDeployBoot` has Default and Update parameter sets for WinRE selection with ADK fallback, plus ADK and ADKUpdate parameter sets for explicitly selecting the ADK WinPE image.

| Parameter          | Type             | Default                                               | Accepted values and behavior                                                                                                                                                                                                      |
| ------------------ | ---------------- | ----------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-Name`            | `String`         | None                                                  | Load an exact saved profile name or name a new persistent profile. Canonical profile names end in `-amd64` or `-arm64`; the suffix is omitted from the friendly media name. When omitted, use `OSDeploy` and a temporary profile. |
| `-Update`          | `String`         | None                                                  | Name an existing canonical profile to reselect its shared content and wallpaper, save it, and build it. Required in the Update and ADKUpdate sets and mutually exclusive with `-Name`.                                            |
| `-Architecture`    | `String`         | Automatic in the Default set; required in the ADK set | Use `amd64` or `arm64`. In the Default set, the value filters the WinRE selector. A selected WinRE image determines the final architecture. With `-Auto`, an omitted value is derived from `PROCESSOR_ARCHITECTURE`.              |
| `-Languages`       | `String[]`       | None                                                  | Add one or more validated ADK languages. Use `*` to enumerate every available language directory except `en-us`. The function processes the `en-us` base packages independently. A selected saved profile replaces this value.    |
| `-SetAllIntl`      | `String`         | None                                                  | Pass a locale to DISM `/Set-AllIntl`. The step is skipped when the value is empty. A selected saved profile replaces this value.                                                                                                  |
| `-SetInputLocale`  | `String`         | None                                                  | Pass an input locale to DISM `/Set-InputLocale`. The step is skipped when the value is empty. A selected saved profile replaces this value.                                                                                       |
| `-SetTimeZone`     | `String`         | Current system timezone                               | Use a timezone returned by `tzutil /l`. The default comes from `tzutil /g`. A selected saved profile replaces this value.                                                                                                         |
| `-SkipAdkPackages` | `Switch`         | Not enabled                                           | Skip ADK optional-component and language-package installation. Other image customization steps still run.                                                                                                                         |
| `-UseAdkWinPE`     | `Switch`         | Not enabled                                           | Use the ADK `winpe.wim`. This switch and `-Architecture` are mandatory in the ADK parameter set and cannot be combined with `-Auto`.                                                                                              |
| `-UpdateUSB`       | `Switch`         | Not enabled                                           | After ISO creation, copy the completed standard media tree to every accessible volume labeled `USB-WinPE`.                                                                                                                        |
| `-Auto`            | `Switch`         | Not enabled                                           | In the Default or Update set, resolve an omitted architecture and select the newest compatible imported WinRE image. New and temporary profiles can still open shared-content and wallpaper selectors.                            |
| `-Options`         | `String[]`       | None                                                  | Use `pwsh`, `dart`, or both. `pwsh` installs PowerShell 7; `dart` installs available Microsoft DaRT content. A saved profile supplies this value unless it is explicitly passed with `-Update`.                                   |
| `-WhatIf`          | Common parameter | Not enabled                                           | Run prerequisite and selection work. Normal builds stop at build-directory creation; `-Update` stops earlier at the profile overwrite. Earlier profile writes and discovery are not suppressed.                                   |
| `-Confirm`         | Common parameter | Not enabled                                           | Prompt before updating an existing profile and before creating the build output directories. The profile prompt applies only with `-Update`; declining either operation stops the command.                                        |

Accepted `-Languages` values are:

```
*, ar-sa, bg-bg, cs-cz, da-dk, de-de, el-gr, en-gb, en-us, es-es,
es-mx, et-ee, fi-fi, fr-ca, fr-fr, he-il, hr-hr, hu-hu, it-it,
ja-jp, ko-kr, lt-lt, lv-lv, nb-no, nl-nl, pl-pl, pt-br, pt-pt,
ro-ro, ru-ru, sk-sk, sl-si, sr-latn-rs, sv-se, th-th, tr-tr,
uk-ua, zh-cn, zh-tw
```

## Examples

### Build from an imported WinRE image

Select an imported WinRE image and shared content, save a new architecture-qualified profile, and build the media:

```powershell
Build-OSDeployBoot -Name 'MyPE'
```

If no WinRE image is selected or available, the function falls back to the Windows ADK WinPE image and derives the architecture from the host.

### Limit WinRE selection to AMD64

Show only imported AMD64 WinRE images in the source selector:

```powershell
Build-OSDeployBoot -Name 'AMD64-Production' -Architecture 'amd64'
```

### Build directly from ADK WinPE

Skip WinRE selection and use the AMD64 `winpe.wim` supplied by the Windows ADK:

```powershell
Build-OSDeployBoot `
	-Name 'ADK-AMD64' `
	-Architecture 'amd64' `
	-UseAdkWinPE
```

ADK WinPE does not support wireless hardware. The function excludes selected driver paths whose leaf names contain `wifi` or `wireless`.

### Select the newest WinRE source automatically

Resolve the architecture from the host and select the newest imported WinRE image for that architecture:

```powershell
Build-OSDeployBoot -Name 'Current-WinRE' -Auto
```

`-Auto` does not make a new or temporary profile unattended. Shared driver, script, startup-profile, and wallpaper selection can still require interaction when matching content exists.

### Update an existing profile

Reselect the profile's shared content and wallpaper, preserve settings that are not explicitly replaced, set its options to PowerShell 7, save it, and build from the newest matching WinRE source:

```powershell
Build-OSDeployBoot `
	-Update 'MyPE-amd64' `
	-Auto `
	-Options 'pwsh'
```

### Add languages and regional settings

Add French and Canadian French language packages, apply French regional settings, use a Canadian French keyboard, and set the timezone to Eastern Time:

```powershell
Build-OSDeployBoot `
	-Name 'French-Canada' `
	-Languages 'fr-fr', 'fr-ca' `
	-SetAllIntl 'fr-CA' `
	-SetInputLocale '0c0c:00001009' `
	-SetTimeZone 'Eastern Standard Time'
```

When `-Name` loads a saved profile, its language and international settings replace values supplied on the command line. With `-Update`, explicitly supplied values replace the corresponding saved settings.

### Skip ADK package installation

Skip all ADK optional-component and language-package installation while retaining the remaining customization steps:

```powershell
Build-OSDeployBoot `
	-Name 'Package-Test' `
	-Architecture 'amd64' `
	-UseAdkWinPE `
	-SkipAdkPackages
```

Use this option only when the source image already contains the components required by the intended WinPE workflow.

### Update a prepared USB volume

Build the media and copy the standard `bootmedia` tree to each accessible volume labeled `USB-WinPE`:

```powershell
Build-OSDeployBoot -Name 'Field-USB' -UpdateUSB
```

The update overwrites matching files but does not purge destination-only files. The function warns and continues when no matching volume is connected.

### Preview the build-directory operation

Resolve the source and profile configuration, display the build context, and stop before creating the build output tree:

```powershell
Build-OSDeployBoot -Name 'Preview' -Auto -WhatIf
```

{% hint style="warning" %}
`-WhatIf` is not a read-only preview. Initialization and profile handling occur before `ShouldProcess`. The function can create or update a build profile, convert module paths in a selected profile to portable tokens, and copy a selected wallpaper into the profile directory before stopping.
{% endhint %}

### Build ARM64 media with explicit settings

Build from the ARM64 ADK source, add German language packages, configure German regional settings, and update a prepared USB volume:

```powershell
Build-OSDeployBoot `
	-Name 'ARM64-DE' `
	-Architecture 'arm64' `
	-UseAdkWinPE `
	-Languages 'de-de' `
	-SetAllIntl 'de-DE' `
	-SetInputLocale '0407:00000407' `
	-SetTimeZone 'W. Europe Standard Time' `
	-UpdateUSB
```

## Source Selection

The Default and Update parameter sets start with an imported WinRE source:

1. With `-Auto`, derive an omitted architecture from `PROCESSOR_ARCHITECTURE`, find imported WinRE images for that architecture, sort by OS version descending, and select the newest image.
2. With `-Architecture`, show the interactive WinRE selector filtered to that architecture.
3. Without either option, show the interactive selector for all supported imported WinRE images.
4. If no WinRE image is selected or available, warn and fall back to ADK WinPE. When the architecture is still unset, derive it from the host.
5. Resolve the architecture-specific ADK paths. The function stops when the architecture or required ADK paths cannot be resolved.

The ADK and ADKUpdate parameter sets bypass WinRE selection and use the architecture-specific ADK `winpe.wim`.

The build folder name uses this format:

```
{Windows build}.{revision}-{architecture}-{Name}
```

For example, a source version of `10.0.26200.1234`, AMD64 architecture, and name `MyPE` produces `26200.1234-amd64-MyPE`. If that directory exists, the function tries `-001`, `-002`, and later suffixes until it finds an unused path.

## Build Profiles

Saved profiles use flat, architecture-qualified directories under:

```
%ProgramData%\OSDeployCore\repository\osdeployboot-profiles\{Name}-{Architecture}\osdeployboot.json
```

An exact `-Name` match loads the saved profile automatically; no profile picker is displayed. The saved profile supplies its architecture, explicit content paths, languages, international settings, timezone, and options. The function rejects a conflicting `-Architecture`, validates configured paths, and converts installed-module paths to these portable tokens when possible:

| Token                       | Resolved module path           |
| --------------------------- | ------------------------------ |
| `${{ OSDeployModulePath }}` | Installed OSDeploy module base |
| `${{ OSDCloudModulePath }}` | Loaded OSDCloud module base    |
| `${{ OSDModulePath }}`      | Loaded OSD module base         |

When `-Name` does not match a saved profile, the function presents shared selectors for compatible drivers, WinPE scripts, media scripts, WinPEStartup profiles, and wallpaper. It then writes a persistent profile to:

```
%ProgramData%\OSDeployCore\repository\osdeployboot-profiles\{Name}-{Architecture}\osdeployboot.json
```

When `-Name` is omitted, the same selectors create a temporary profile for the build. The function removes that profile after normal completion, cancellation, or a terminating error; generated build output remains. Shared scripts can come from the OSDeploy repository or the installed OSDeploy, OSDCloud, and OSD module roots. Architecture filtering excludes script names associated with the opposite architecture.

`-Update` requires an existing canonical profile. It reselects shared drivers, WinPE scripts, media scripts, WinPEStartup profiles, and wallpaper. Driver, script, and startup-profile selections replace their saved values; canceling those selectors clears the corresponding value. Canceling wallpaper selection preserves the existing wallpaper. Languages, international settings, timezone, and options are preserved unless explicitly passed.

## Profile-Local Content

Keep portable content beside `osdeployboot.json` to include it without storing additional absolute paths in the profile:

```
{profile}\
|-- osdeployboot.json
|-- build-mediascript\
|-- build-winpeapp\
|-- build-winpedrivers\
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
| WinPE drivers         | Add explicit paths first. When recursive `*.inf` files exist under local `build-winpedrivers`, append that directory once and let DISM add its drivers recursively.                                                                                                         |
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

For a normal or new-profile build, the function calls `ShouldProcess` before creating the build output directories. Before that call, it performs requirement checks, initializes OSDeploy Core state, selects the source and content, loads or writes a profile, resolves wallpaper, populates `$global:BuildMedia`, displays the configuration, and waits five seconds.

With `-Update`, an earlier `ShouldProcess` call gates overwriting the existing profile. `-WhatIf -Update` stops at that profile operation, before wallpaper processing, build-context creation, the five-second delay, or build-directory handling. If the profile update is approved, the later build-directory gate still applies.

With `-WhatIf` in the other parameter sets, the function stops at the directory operation. It does not create or service the build media, mount an image, create an ISO, or update USB media. New or temporary profile directories, profile JSON, selected wallpaper, and automatic token migration can occur before that gate.

With `-Confirm`, a normal build prompts at the directory operation. An update can prompt first for the profile overwrite and again for the build directories. Declining either operation returns immediately.

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

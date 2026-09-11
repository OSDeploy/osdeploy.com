---
description: >-
  Agent instructions for generating a valid Build-OSDeployBoot command from a
  user's requested source, profile, architecture, and build options.
icon: brackets-curly
---

# Build-OSDeployBoot

Use these instructions to translate a user's requested OSDeploy Boot configuration into the smallest valid `Build-OSDeployBoot` command line.

{% hint style="info" %}
Preserve automatic discovery. Omit a parameter when the user accepts the function default, source picker, content selectors, profile value, or fallback behavior.
{% endhint %}

## Agent contract

Generate a command line only. Do not execute `Build-OSDeployBoot`, run a preview, install prerequisites, create a profile, or change the user's system as part of this workflow.

Follow these rules:

* Use only parameters documented on this page.
* Add only parameters required by the user's request.
* Never combine `-Auto` with `-UseAdkWinPE`.
* For an ADK WinPE build without `-ProfileName`, require `-Architecture`.
* For a named-profile build, use the exact canonical profile name supplied by the user. Do not invent or abbreviate it.
* Omit `-Architecture` when the named profile should supply it. If the user explicitly provides both values, require the architecture to match the profile.
* Do not add `-WhatIf` unless the user asks for a preview command.
* Warn that `-WhatIf` is not read-only because initialization, selection, and a recent profile write can occur before the build-directory gate.
* Do not add `-Confirm` unless the user requests PowerShell confirmation at the build-directory operation.
* Quote string values with single quotes and escape an embedded single quote by doubling it.
* Render multiple `-Languages` or `-Options` values as a comma-separated PowerShell array.
* Return one runnable PowerShell command and a concise explanation of automatic or interactive behavior that remains.
* If required information is missing, ask one concise group of questions instead of producing a guessed command.

## Requirements

The generated command is intended for an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later.

Execution requires:

| Requirement | Required state |
| --- | --- |
| Module | Current OSDeploy module exporting `Build-OSDeployBoot` |
| License | Valid Recast Software Community License for direct invocation |
| OSDeploy Core | Initialized and populated for the requested workflow |
| Windows ADK | Windows ADK and WinPE add-on installed |
| OSDCloud | Version `26.7.25.2` or later |
| Utility | `curl.exe` available in `PATH` |
| Session | Administrator rights |

Do not test, install, update, or repair these requirements when the user asks only for a command line.

## Collect the request

Identify the source mode first, then collect only explicit overrides.

| User requirement | Parameter decision |
| --- | --- |
| Choose an imported WinRE interactively | Omit `-Auto` and `-UseAdkWinPE` |
| Use the newest imported WinRE automatically | Add `-Auto`; add `-Architecture` only when the user wants to pin it |
| Use ADK WinPE directly | Add `-UseAdkWinPE`; require `-Architecture` unless a named profile supplies it |
| Load a saved build profile | Add the exact `-ProfileName` |
| Use the profile's architecture and settings | Omit matching command-line overrides |
| Limit interactive WinRE selection by architecture | Add `-Architecture 'amd64'` or `-Architecture 'arm64'` |
| Add language packages | Add validated values to `-Languages` |
| Configure regional or keyboard settings | Add the requested `-SetAllIntl` and `-SetInputLocale` values |
| Use the current system timezone | Omit `-SetTimeZone` |
| Use a specific timezone | Add an exact timezone returned by `tzutil /l` |
| Skip ADK optional-component and language packages | Add `-SkipAdkPackages` |
| Add PowerShell 7 or DaRT | Add `pwsh`, `dart`, or both to `-Options` |
| Update prepared USB media after the build | Add `-UpdateUSB` |
| Stop before build-directory creation | Add `-WhatIf` and include the side-effect warning |
| Prompt at build-directory creation | Add `-Confirm` |

When the request is ambiguous, ask only the questions needed to select a parameter set or validate an explicit value. Common unresolved choices are:

1. Imported WinRE selection, newest WinRE with `-Auto`, or ADK WinPE.
2. `amd64` or `arm64` when the selected mode requires an explicit architecture.
3. The exact canonical profile name when the user wants a saved profile.
4. Exact language, locale, keyboard, timezone, option, or USB-update preferences requested by the user.

## Parameter sets

Choose exactly one compatible parameter set.

| Parameter set | Use when | Required choices |
| --- | --- | --- |
| Default | Build without a named profile from WinRE selection or `-Auto`, with ADK fallback | None |
| Profile | Load a named profile and use WinRE selection or `-Auto`, with ADK fallback | `-ProfileName` |
| ADK | Build directly from the ADK `winpe.wim` without a named profile | `-UseAdkWinPE` and `-Architecture` |
| ADKProfile | Load a named profile and build directly from the ADK `winpe.wim` | `-ProfileName` and `-UseAdkWinPE`; add `-Architecture` only when explicitly requested and matching |

Use these compatibility rules:

* `-Auto` is valid only in the Default and Profile sets.
* `-UseAdkWinPE` selects the ADK or ADKProfile set and cannot be combined with `-Auto`.
* `-ProfileName` selects a Profile or ADKProfile set.
* All remaining function parameters are available in every parameter set.

## Parameter reference

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | None | Exact name of an existing architecture-specific profile, such as `Contoso-amd64`. The profile supplies architecture, content, and saved settings. Explicit command-line settings override saved values for this build only. |
| `-Architecture` | `String` | Automatic | `amd64` or `arm64`. Filters WinRE selection, pins `-Auto`, or selects ADK paths. Required for ADK mode when no profile supplies it. |
| `-Languages` | `String[]` | Profile value or none | One or more validated ADK language identifiers. Use `*` for all additional available languages. An explicit value overrides a named profile for this build only. |
| `-SetAllIntl` | `String` | Profile value or none | Regional setting passed to the WinPE international-settings step. |
| `-SetInputLocale` | `String` | Profile value or none | Input locale passed to WinPE servicing. |
| `-SetTimeZone` | `String` | Profile value or current system timezone | Exact timezone accepted by `tzutil /l`. |
| `-SkipAdkPackages` | `Switch` | Not enabled | Skip ADK optional-component and language-package installation. Other customization still runs. |
| `-UseAdkWinPE` | `Switch` | Not enabled | Use the architecture-specific ADK `winpe.wim` instead of starting with imported WinRE selection. |
| `-UpdateUSB` | `Switch` | Not enabled | Run the final update step for accessible partitions labeled `USB-WinPE`. |
| `-Auto` | `Switch` | Not enabled | Select the newest imported WinRE for the resolved architecture without the source picker. Fall back to ADK WinPE when no compatible WinRE exists. |
| `-Options` | `String[]` | Profile value or none | `pwsh`, `dart`, or both. |
| `-WhatIf` | Common parameter | Not enabled | Stop at build-directory creation after prerequisite checks, source and content selection, configuration, and possible recent-profile writes. |
| `-Confirm` | Common parameter | Not enabled | Request confirmation before creating the build output directories. |

Accepted `-Languages` values:

```text
*, ar-sa, bg-bg, cs-cz, da-dk, de-de, el-gr, en-gb, en-us, es-es,
es-mx, et-ee, fi-fi, fr-ca, fr-fr, he-il, hr-hr, hu-hu, it-it,
ja-jp, ko-kr, lt-lt, lv-lv, nb-no, nl-nl, pl-pl, pt-br, pt-pt,
ro-ro, ru-ru, sk-sk, sl-si, sr-latn-rs, sv-se, th-th, tr-tr,
uk-ua, zh-cn, zh-tw
```

## Build the command

Start with the smallest command that satisfies the request:

```powershell
Build-OSDeployBoot
```

Use this generation order:

1. Start with `Build-OSDeployBoot`.
2. Add `-ProfileName` when the user specifies a saved profile.
3. Add one source strategy: `-Auto`, `-UseAdkWinPE`, or neither for interactive WinRE selection.
4. Add `-Architecture` only when required or explicitly requested.
5. Add explicit language, international, timezone, package, feature, and USB preferences.
6. Add `-WhatIf` or `-Confirm` only when requested.
7. Check the completed command against the selected parameter set.

Prefer a single line. Use PowerShell line continuations only when several explicit settings make the command difficult to read.

## Generation examples

### Interactive WinRE selection

User intent: Choose an imported WinRE image and build with the normal content selectors.

```powershell
Build-OSDeployBoot
```

### Newest WinRE for the host architecture

User intent: Automatically use the newest compatible imported WinRE image.

```powershell
Build-OSDeployBoot -Auto
```

The command derives the architecture from the host. Shared content and wallpaper selectors can still appear when matching content exists.

### Named profile with a temporary option override

User intent: Load `Contoso-amd64`, automatically select the newest matching WinRE, and add PowerShell 7 for this build.

```powershell
Build-OSDeployBoot -ProfileName 'Contoso-amd64' -Auto -Options 'pwsh'
```

The profile supplies its architecture and content. The explicit option applies only to this build.

### Direct ARM64 ADK build

User intent: Build directly from the ARM64 ADK image and skip ADK package installation.

```powershell
Build-OSDeployBoot -Architecture 'arm64' -UseAdkWinPE -SkipAdkPackages
```

### Multiple languages and options

User intent: Build from AMD64 ADK WinPE with French and Canadian French language packages, PowerShell 7, and DaRT.

```powershell
Build-OSDeployBoot -Architecture 'amd64' -UseAdkWinPE -Languages 'fr-fr', 'fr-ca' -Options 'pwsh', 'dart'
```

### Preview command

User intent: Generate a preview command that automatically selects the newest WinRE.

```powershell
Build-OSDeployBoot -Auto -WhatIf
```

{% hint style="warning" %}
`-WhatIf` is not read-only. Without `-ProfileName`, the function can initialize repository paths, display selectors, write `recent-amd64.json` or `recent-arm64.json`, and stage temporary profile content before it stops at build-directory creation.
{% endhint %}

## Automatic and interactive behavior

When neither `-Auto` nor `-UseAdkWinPE` is present, the command displays the imported WinRE selector. `-Architecture` limits that selector when supplied. If no WinRE source is selected or available, the function warns and falls back to ADK WinPE.

When `-Auto` is present, the function derives an omitted architecture from `PROCESSOR_ARCHITECTURE`, selects the newest imported WinRE for that architecture, and falls back to ADK WinPE when no match exists.

When `-ProfileName` is omitted, the function displays shared selectors for compatible drivers, scripts, WinPEStartup profiles, and wallpaper, then writes the resolved configuration to the matching recent architecture profile. When `-ProfileName` is present, the function loads that profile without shared-content or wallpaper selectors and does not modify the named profile.

Before creating build directories, the function displays the configuration and waits five seconds. A normal command then builds media under `C:\ProgramData\OSDeployCore\boot`. `-UpdateUSB` also targets accessible partitions labeled `USB-WinPE` after the build.

## Response format

Return the generated command in a `powershell` code block. Follow it with no more than three concise statements covering:

* Why the selected parameter set is valid.
* Which source, architecture, profile, or settings remain automatic or interactive.
* The `-WhatIf` side-effect warning when applicable.

Do not claim that the command was validated against the user's installed profiles, timezones, cached images, ADK files, DaRT content, or USB media unless the user separately asked for environment inspection and that inspection was completed.

For workflow requirements, use [Build an OSDeploy Boot Image](../../guide/basic/build-osdeployboot.md). For complete behavior and examples, use the [detailed Build-OSDeployBoot guide](../../guide/cmdlets/build-osdeployboot.md) or the [command reference](../../command-reference/osdeploy/build-osdeployboot.md).

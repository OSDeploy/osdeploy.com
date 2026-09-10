# Build-OSDeployBoot

Builds customized WinPE boot media from an imported WinRE or Windows ADK source.

| Property | Value |
| --- | --- |
| Module | OSDeploy |
| Platform | Windows 11 25H2 build 26200 or later (amd64 / arm64) |
| Requires | PowerShell 7.6, Windows ADK and WinPE add-on, OSDCloud 26.7.25.2, Administrator rights, valid license |
| Output | None; sets `$global:BuildMedia` as process state |

## Syntax

```powershell
# Default
Build-OSDeployBoot [-Architecture <String>] [-Languages <String[]>]
    [-SetAllIntl <String>] [-SetInputLocale <String>] [-SetTimeZone <String>]
    [-SkipAdkPackages] [-UpdateUSB] [-Auto] [-Options <String[]>] [-WhatIf] [-Confirm]

# Profile
Build-OSDeployBoot -ProfileName <String> [-Architecture <String>]
    [-Languages <String[]>] [-SetAllIntl <String>] [-SetInputLocale <String>]
    [-SetTimeZone <String>] [-SkipAdkPackages] [-UpdateUSB] [-Auto]
    [-Options <String[]>] [-WhatIf] [-Confirm]

# ADK
Build-OSDeployBoot -Architecture <String> -UseAdkWinPE [-Languages <String[]>]
    [-SetAllIntl <String>] [-SetInputLocale <String>] [-SetTimeZone <String>]
    [-SkipAdkPackages] [-UpdateUSB] [-Options <String[]>] [-WhatIf] [-Confirm]

# ADKProfile
Build-OSDeployBoot -ProfileName <String> -UseAdkWinPE [-Architecture <String>]
    [-Languages <String[]>] [-SetAllIntl <String>] [-SetInputLocale <String>]
    [-SetTimeZone <String>] [-SkipAdkPackages] [-UpdateUSB]
    [-Options <String[]>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | Profile sets | Exact name of an existing architecture-specific profile. The profile is read but not modified. |
| `-Architecture` | `String` | ADK set | `amd64` or `arm64`; filters WinRE selection or selects ADK paths. |
| `-Languages` | `String[]` | No | Valid ADK language identifiers or `*`. |
| `-SetAllIntl` | `String` | No | WinPE international-settings value. |
| `-SetInputLocale` | `String` | No | WinPE input locale. |
| `-SetTimeZone` | `String` | No | Value accepted by `tzutil /l`; defaults to the current timezone. |
| `-SkipAdkPackages` | `Switch` | No | Skip ADK optional-component and language packages. |
| `-UseAdkWinPE` | `Switch` | ADK sets | Use ADK `winpe.wim`; cannot be combined with `-Auto`. |
| `-UpdateUSB` | `Switch` | No | Run the final update step for `USB-WinPE` partitions. |
| `-Auto` | `Switch` | WinRE sets | Select the newest compatible WinRE source, with ADK fallback. |
| `-Options` | `String[]` | No | `pwsh`, `dart`, or both. |
| `-WhatIf` | `Switch` | No | Stop at build-directory creation after configuration; a recent profile snapshot can already be written. |
| `-Confirm` | `Switch` | No | Confirm build-directory creation. |

## Examples

```powershell
Build-OSDeployBoot
```

```powershell
Build-OSDeployBoot -ProfileName 'Contoso-amd64' -Options 'pwsh'
```

```powershell
Build-OSDeployBoot -Architecture 'arm64' -UseAdkWinPE
```

When `-ProfileName` is omitted, configuration is written to `recent-amd64.json` or `recent-arm64.json`. Named profiles are created and maintained with the BootProfilePreview commands.

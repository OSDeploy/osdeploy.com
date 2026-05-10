# Build-OSDeployBootMedia

Builds a customized WinPE boot image from a WinRE or ADK WinPE source.

| Property     | Value                                                                        |
|--------------|------------------------------------------------------------------------------|
| Module       | OSDeploy                                                                     |
| Platform     | Windows 11 25H2+ (amd64 / arm64)                                            |
| Requires     | PowerShell 7.6, Windows ADK, OSDCloud module 26.4.17.1+, Run as Administrator |
| Output path  | `%ProgramData%\OSDeployCore\boot-media`                                      |

## Description

Creates a bootable WinPE image from an imported WinRE image or the Windows ADK WinPE. The function applies ADK optional components, WinPE drivers, PowerShell module updates, WinPE applications, console settings, wallpaper, and user scripts. Output is written to `%ProgramData%\OSDeployCore\boot-media`.

By default the function selects an imported WinRE source. Use `-UseAdkWinPE` to build from the ADK WinPE instead.

## Syntax

```powershell
# Default parameter set — uses an imported WinRE source
Build-OSDeployBootMedia -Name <String> [-Architecture <String>]
    [-Languages <String[]>] [-SetAllIntl <String>] [-SetInputLocale <String>]
    [-SetTimeZone <String>] [-SkipAdkPackages] [-UpdateUSB] [-WhatIf] [-Confirm]

# ADK parameter set — uses the Windows ADK winpe.wim
Build-OSDeployBootMedia -Name <String> -Architecture <String>
    [-Languages <String[]>] [-SetAllIntl <String>] [-SetInputLocale <String>]
    [-SetTimeZone <String>] [-SkipAdkPackages] [-UseAdkWinPE] [-UpdateUSB]
    [-WhatIf] [-Confirm]
```

## Parameters

| Parameter         | Type       | Required               | Description                                                                                           |
|-------------------|------------|------------------------|-------------------------------------------------------------------------------------------------------|
| `-Name`           | `String`   | Yes (all sets)         | Friendly name for the build. Used to label the output folder.                                         |
| `-Architecture`   | `String`   | Optional / Yes (ADK)   | Processor architecture: `amd64` or `arm64`. Required with `-UseAdkWinPE`. Default: `amd64`.          |
| `-Languages`      | `String[]` | No                     | ADK language packs to add. Default: `en-us`. Use `*` to add all available languages.                 |
| `-SetAllIntl`     | `String`   | No                     | Sets all international settings to the specified locale. Default: `en-us`.                            |
| `-SetInputLocale` | `String`   | No                     | Sets the default input locale in WinPE. Default: `en-us`.                                            |
| `-SetTimeZone`    | `String`   | No                     | Sets the WinPE timezone. Default: the current system timezone (from `tzutil /g`).                     |
| `-SkipAdkPackages`| `Switch`   | No                     | Skips adding ADK optional component packages. Useful for quick testing.                               |
| `-UseAdkWinPE`    | `Switch`   | Yes (ADK set)          | Uses the ADK `winpe.wim` instead of an imported WinRE source. Requires `-Architecture`.               |
| `-UpdateUSB`      | `Switch`   | No                     | Copies the completed media to any connected USB partition labeled `USB-WinPE` after the build.        |

## Examples

```powershell
# Build from an imported WinRE source with the default name
Build-OSDeployBootMedia -Name 'MyPE'
```

```powershell
# Build from the ADK WinPE for amd64, skipping optional components
Build-OSDeployBootMedia -Name 'TestBuild' -Architecture 'amd64' -UseAdkWinPE -SkipAdkPackages
```

```powershell
# Build for arm64 and copy to any connected USB-WinPE drive
Build-OSDeployBootMedia -Name 'ARM64-PE' -Architecture 'arm64' -UpdateUSB
```

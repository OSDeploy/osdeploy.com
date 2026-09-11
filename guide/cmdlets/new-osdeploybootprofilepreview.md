---
description: Create a persistent architecture-specific OSDeploy Boot profile without building media.
---

# New-OSDeployBootProfilePreview

`New-OSDeployBootProfilePreview` creates a reusable OSDeploy Boot profile from a selected cached Windows Recovery image. It derives the architecture, selects shared Boot-Assets content, and writes portable settings to `osdeployboot.json` without building media.

## Requirements

Run the command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. A cached Windows Recovery image and write access to `%ProgramData%\OSDeployCore\boot-assets` are required. An interactive console is required when multiple recovery images are available.

The command does not use the shared OSDeploy license gate.

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | None | Required. Specify a base name such as `Contoso` or a canonical name ending in `-amd64` or `-arm64`. A supplied suffix must match the selected recovery image. |
| `-Languages` | `String[]` | None | Save one or more supported ADK language identifiers. Use `*` for all additional languages during a later build. |
| `-SetAllIntl` | `String` | None | Save the WinPE international-settings value. |
| `-SetInputLocale` | `String` | None | Save the WinPE input locale. |
| `-SetTimeZone` | `String` | Current system timezone | Save a timezone accepted by `tzutil /l`. |
| `-Options` | `String[]` | None | Save `pwsh`, `dart`, or both. |
| `-WhatIf` | Common parameter | Not enabled | Report profile creation without displaying selectors or writing files. |
| `-Confirm` | Common parameter | Not enabled | Confirm the profile creation operation. |

## Examples

### Create a profile

Select a cached recovery image and shared content, then create an architecture-qualified profile:

```powershell
New-OSDeployBootProfilePreview -ProfileName 'Contoso'
```

An AMD64 selection creates `Contoso-amd64`; an ARM64 selection creates `Contoso-arm64`.

### Save PowerShell as an option

```powershell
New-OSDeployBootProfilePreview -ProfileName 'Contoso' -Options 'pwsh'
```

### Preview profile creation

```powershell
New-OSDeployBootProfilePreview -ProfileName 'Contoso' -WhatIf
```

`-WhatIf` stops before recovery-image and shared-content selection.

## Profile Creation

Profiles are stored under:

```text
%ProgramData%\OSDeployCore\boot-assets\osdeployboot-profiles\<Name>-<Architecture>\
```

The selected recovery image controls the profile architecture. Existing profile directories are not overwritten. If no recovery image is selected, the command warns and creates nothing.

The command selects shared drivers, WinPE scripts, media scripts, WinPEStartup profiles, and wallpaper. Paths beneath OSDeployCore are saved with the portable `${{ OSDeployCore }}` token. The selected wallpaper is copied to the profile root as `wallpaper.jpg`.

Profile-local content can be added beside `osdeployboot.json` under `boot-mediascript`, `boot-winpescript`, the matching `winpedrivers-<architecture>` directory, `WinPEStartup\profiles`, and `WinPEStartup\assets`.

## WhatIf and Confirmation

The command calls `ShouldProcess` before displaying recovery-image or content selectors and before creating the profile directory. Declining confirmation or using `-WhatIf` creates no profile and displays no selectors.

## Output

The command writes no object to the pipeline.

Build the saved profile with [Build-OSDeployBoot](build-osdeployboot.md). See the compact [New-OSDeployBootProfilePreview command reference](../../command-reference/osdeploy/new-osdeploybootprofilepreview.md) for syntax lookup.

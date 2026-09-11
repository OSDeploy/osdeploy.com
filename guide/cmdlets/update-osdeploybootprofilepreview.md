---
description: Update the selections and settings of an existing OSDeploy Boot profile.
---

# Update-OSDeployBootProfilePreview

`Update-OSDeployBootProfilePreview` updates an existing profile without building media. It preserves omitted regional settings and options, while rerunning the shared content and wallpaper selectors.

## Requirements

Run the command from an elevated PowerShell 7.6 or later session with write access to the selected profile directory. The exact canonical profile name must identify a directory containing `osdeployboot.json` under `%ProgramData%\OSDeployCore\boot-assets\osdeployboot-profiles`.

A valid Recast Software Community License is required when the command is called directly. The command displays registration guidance and returns when no valid license is available.

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | None | Required. Specify the exact canonical profile name. Values tab-complete from readable profile directories. |
| `-Languages` | `String[]` | Existing value | Replace the saved ADK languages. Use `*` for all additional languages during a later build. |
| `-SetAllIntl` | `String` | Existing value | Replace the saved WinPE international-settings value. |
| `-SetInputLocale` | `String` | Existing value | Replace the saved WinPE input locale. |
| `-SetTimeZone` | `String` | Existing value | Replace the saved timezone with a value accepted by `tzutil /l`. |
| `-Options` | `String[]` | Existing value | Replace saved options with `pwsh`, `dart`, or both. |
| `-WhatIf` | Common parameter | Not enabled | Validate the license and locate the profile, then report the update without displaying content selectors or modifying files. |
| `-Confirm` | Common parameter | Not enabled | Confirm the profile update before initialization and content selection. |

## Examples

### Reselect shared content

```powershell
Update-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64'
```

Regional settings and options remain unchanged. Shared content selections are replaced.

### Replace saved options

```powershell
Update-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64' -Options 'pwsh'
```

### Preview an update

```powershell
Update-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64' -WhatIf
```

## Selection and Preservation

The profile architecture is read from `osdeployboot.json` and must be `amd64` or `arm64`. The command reselects shared drivers, WinPE scripts, media scripts, WinPEStartup profiles, and wallpaper.

Canceling the driver, script, media-script, or startup-profile selector clears the corresponding saved value. Canceling wallpaper selection preserves the current profile-root `wallpaper.jpg`.

Omitted `Languages`, `SetAllIntl`, `SetInputLocale`, `SetTimeZone`, and `Options` values are read from the existing profile. Explicit values replace them. Paths beneath OSDeployCore are stored with the portable `${{ OSDeployCore }}` token.

Core initialization can migrate legacy Boot-Assets content and profile paths before the final profile is reloaded. Existing Boot-Assets content takes precedence during general `repository` and `OSDRepo` migration; unresolved conflicts remain at the legacy source with a warning.

## WhatIf and Confirmation

The license check and initial profile lookup occur before `ShouldProcess`. The shared selectors and profile write occur only after the update operation is approved. `-WhatIf` and a declined confirmation do not modify the profile.

## Output

The command writes no object to the pipeline.

Build the updated profile with [Build-OSDeployBoot](build-osdeployboot.md). See the compact [Update-OSDeployBootProfilePreview command reference](../../command-reference/osdeploy/update-osdeploybootprofilepreview.md) for syntax lookup.

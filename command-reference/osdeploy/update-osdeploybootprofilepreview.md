# Update-OSDeployBootProfilePreview

Updates an existing OSDeploy Boot profile without building media.

Requires a valid Recast Software Community License when called directly.

## Syntax

```powershell
Update-OSDeployBootProfilePreview [-ProfileName] <String> [[-Languages] <String[]>]
    [[-SetAllIntl] <String>] [[-SetInputLocale] <String>] [[-SetTimeZone] <String>]
    [[-Options] <String[]>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | Yes | Exact canonical name of an existing profile. Values tab-complete from profile directories containing `osdeployboot.json`. |
| `-Languages` | `String[]` | No | Replace saved ADK language identifiers. |
| `-SetAllIntl` | `String` | No | Replace the saved international-settings value. |
| `-SetInputLocale` | `String` | No | Replace the saved WinPE input locale. |
| `-SetTimeZone` | `String` | No | Replace the saved timezone with a value accepted by `tzutil /l`. |
| `-Options` | `String[]` | No | Replace saved options with `pwsh`, `dart`, or both. |
| `-WhatIf` | `Switch` | No | Show the update without displaying selectors or modifying the profile. |
| `-Confirm` | `Switch` | No | Confirm the profile update operation. |

## Example

```powershell
Update-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64' -Options 'pwsh'
```

Unspecified regional settings and options are preserved. Shared content selectors run again; canceling driver or script selectors clears those values, while canceling wallpaper selection preserves the current wallpaper. The command returns no pipeline object.

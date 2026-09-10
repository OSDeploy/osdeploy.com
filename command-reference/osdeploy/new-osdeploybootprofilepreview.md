# New-OSDeployBootProfilePreview

Creates a persistent architecture-specific OSDeploy Boot profile without building media.

## Syntax

```powershell
New-OSDeployBootProfilePreview [-ProfileName] <String> [[-Languages] <String[]>]
    [[-SetAllIntl] <String>] [[-SetInputLocale] <String>] [[-SetTimeZone] <String>]
    [[-Options] <String[]>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | Yes | Base or canonical profile name. A base name receives the selected `-amd64` or `-arm64` suffix. |
| `-Languages` | `String[]` | No | ADK language identifiers to save; use `*` for all additional languages. |
| `-SetAllIntl` | `String` | No | International-settings value to save. |
| `-SetInputLocale` | `String` | No | WinPE input locale to save. |
| `-SetTimeZone` | `String` | No | Timezone validated against `tzutil /l`; defaults to the current timezone. |
| `-Options` | `String[]` | No | `pwsh`, `dart`, or both. |
| `-WhatIf` | `Switch` | No | Show profile creation without displaying selectors or writing files. |
| `-Confirm` | `Switch` | No | Confirm the profile creation operation. |

## Example

```powershell
New-OSDeployBootProfilePreview -ProfileName 'Contoso' -Options 'pwsh'
```

The command prompts for cached WinRE, derives the architecture, selects shared content, and writes portable paths to `osdeployboot.json`. It returns no pipeline object. Use `Build-OSDeployBoot -ProfileName` to build the profile.

# Update-OSDeployCoreRE

Exports Windows Recovery Environment images and stages supporting Windows OS content from verified Enterprise ESD files.

| Property | Value |
| --- | --- |
| Module | OSDeploy |
| Platform | Windows 11 25H2 build 26200 or later (amd64 / arm64) |
| Requires | PowerShell 7.6, Administrator rights, DISM cmdlets, `robocopy.exe`, valid license |
| Output | `System.IO.DirectoryInfo` for each completed export |

## Syntax

```powershell
Update-OSDeployCoreRE [[-Architecture] <String>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-Architecture` | `String` | No | `amd64` or `arm64`. Omission processes every verified architecture in the newest catalog cache. |
| `-WhatIf` | `Switch` | No | Verify sources and resolve destinations without exporting content. Core initialization still occurs. |
| `-Confirm` | `Switch` | No | Confirm each destination export. |

## Examples

```powershell
Update-OSDeployCoreRE
```

```powershell
Update-OSDeployCoreRE -Architecture 'amd64' -WhatIf
```

The command uses ESD indexes 1, 2, and 3 for setup media, WinPE, and WinSetup, exports the first Enterprise non-N image as `install.wim`, then extracts WinRE and supporting content. A source is skipped only when both matching Windows OS and Windows RE destinations exist. Skipped exports return no object.

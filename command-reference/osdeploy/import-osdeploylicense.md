# Import-OSDeployLicense

Imports and reconciles a Recast Software license from a `.license2` file or ZIP archive.

| Property | Value |
| --- | --- |
| Module | OSDeploy |
| Platform | Windows 11 (amd64 / arm64) |
| Requires | PowerShell 7.6; write access to the ProgramData license directory |
| Output | `System.Management.Automation.PSCustomObject` after a successful import |

## Syntax

```powershell
Import-OSDeployLicense [-LicenseFile] <String> [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-LicenseFile` | `String` | Yes | Existing `.license2` file or ZIP archive. ZIP archives are searched recursively. |
| `-WhatIf` | `Switch` | No | Preview filesystem operations controlled by `ShouldProcess`; the separate import confirmation still appears. |
| `-Confirm` | `Switch` | No | Request confirmation for destination creation, removals, and copy operations. |

## Examples

```powershell
Import-OSDeployLicense -LicenseFile 'C:\Downloads\CommunityLicense.license2'
```

```powershell
Import-OSDeployLicense -LicenseFile 'C:\Downloads\CommunityLicense.zip'
```

The command retains the newest license in each renewal group, removes exact or superseded duplicates, and adds a numeric filename suffix only when required to avoid a collision. After a successful copy, it returns the valid license object produced by `Show-OSDeployLicense`.

# Show-OSDeployLicense

Returns the selected valid Recast Software license or displays registration guidance.

| Property | Value |
| --- | --- |
| Module | OSDeploy |
| Platform | Windows 11 (amd64 / arm64) |
| Requires | PowerShell 7.6 |
| Output | `System.Management.Automation.PSCustomObject`, or no object when no valid license is found |

## Syntax

```powershell
Show-OSDeployLicense
```

## Parameters

This command has no command-specific parameters.

## Example

```powershell
$license = Show-OSDeployLicense
$license | Format-List
```

The command inspects the standard Recast Software license locations, validates candidates, and returns the selected valid license. When no valid candidate exists, it writes registration guidance instead of returning a license object.

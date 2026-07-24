# Get-WinREPartition

Retrieves the Windows Recovery Environment partition information

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns the partition information for the Windows Recovery Environment (WinRE) WIM file.
This function must be run in Windows.

## Syntax

```powershell
Get-WinREPartition [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-WinREPartition
Returns the WinRE partition information
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

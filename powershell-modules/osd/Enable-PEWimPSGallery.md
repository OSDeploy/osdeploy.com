# Enable-PEWimPSGallery

Enables PowerShell Gallery functionality in a WinPE WIM file

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Mounts a WinPE WIM file and configures it to support PowerShell Gallery functionality by modifying registry settings and environment variables.

## Syntax

```powershell
Enable-PEWimPSGallery [-ImagePath] <String[]> [[-Index] <UInt32>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ImagePath` | `String[]` | True | Full path to the WinPE WIM file to modify. This parameter is mandatory and accepts pipeline input. |
| `-Index` | `UInt32` | False | Index of the WIM to mount. Default is 1 |

## Examples

### Example
```powershell
Enable-PEWimPSGallery -ImagePath 'C:\WinPE\winpe.wim'
Enables PowerShell Gallery in the specified WIM file
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

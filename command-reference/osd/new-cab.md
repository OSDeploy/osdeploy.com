# New-CAB

Creates a CAB file from a Directory

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates a CAB file from a Directory

## Syntax

```powershell
New-CAB [-SourceDirectory] <String> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-SourceDirectory` | `String` | True | Directory to create the CAB from |

## Examples

### Example
```powershell
New-CAB -SourceDirectory C:\DeploymentShare\OSDeploy\OSConfig
Creates LZX High Compression CAB from of C:\DeploymentShare\OSDeploy\OSConfig
Saves file in Parent Directory C:\DeploymentShare\OSDeploy\OSConfig.cab
```

## Related

* [https://www.osdeploy.com/](https://www.osdeploy.com/)

# Show-RegistryXML

Displays registry entries from all RegistryXML files in the Source Directory

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Displays registry entries from all RegistryXML files in the Source Directory

## Syntax

```powershell
Show-RegistryXML [-SourceDirectory] <String> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-SourceDirectory` | `String` | True | Directory to search for XML files |

## Examples

### Example
```powershell
Show-RegistryXML -SourceDirectory C:\DeploymentShare\OSDeploy\OSConfig\LocalPolicy\ImportGPO
Displays all RegistryXML entries found in Source Directory
```

## Related

* [https://www.osdeploy.com/](https://www.osdeploy.com/)

# Convert-EsdToFolder

Expands an ESD file into a Windows setup folder structure.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Converts an ESD image into folder media by expanding setup media and
exporting boot and install images to the destination structure.

## Syntax

```powershell
Convert-EsdToFolder [-esdFullName] <String> [[-folderFullName] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-esdFullName` | `String` | True | Full path to the source ESD file. |
| `-folderFullName` | `String` | False | Destination folder path. If omitted, a folder is created next to the ESD. |

## Examples

### Example
```powershell
Convert-EsdToFolder -esdFullName 'C:\Media\install.esd'
Expands the ESD into a setup folder beside the source file.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Save-EnablementPackage

Downloads a matching Windows enablement package.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves an enablement package for the requested build and architecture, verifies connectivity, and downloads the package to the specified directory.

## Syntax

```powershell
Save-EnablementPackage [[-DownloadPath] <String>] [[-OSBuild] <String>] [[-OSArch] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Destination directory where the enablement package file is saved. |
| `-OSBuild` | `String` | False | Target Windows release build used to select the enablement package. |
| `-OSArch` | `String` | False | Target operating system architecture used to select the enablement package. |

## Examples

### Example
```powershell
Save-EnablementPackage -DownloadPath C:\Temp -OSBuild 22H2 -OSArch x64
Downloads the latest matching x64 enablement package for 22H2 to C:\Temp.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

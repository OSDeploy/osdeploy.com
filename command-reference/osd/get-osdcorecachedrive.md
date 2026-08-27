# Get-OSDCoreCacheDrive

Returns OSDCloud cache drive metadata from local file system drives.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Enumerates mounted file system drives that contain an OSDCloud cache path
and returns only USB, DriveRoot, VolumeLabel, and VolumeUniqueId properties.

## Syntax

```powershell
Get-OSDCoreCacheDrive [[-Include] <String[]>] [[-Exclude] <String[]>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Include` | `String[]` | False | Optional list of drive letters to include when searching for OSDCloud cache paths. Accepts values such as 'C', 'D:', or 'E:\'. When omitted, all mounted file system drive letters are considered. |
| `-Exclude` | `String[]` | False | Optional list of drive letters to exclude when searching for OSDCloud cache paths. Accepts values such as 'C', 'D:', or 'E:\'. Excluded drives are skipped even when they are also present in Include. |

## Examples

### Example
```powershell
Get-OSDCoreCacheDrive
```

Returns OSDCloud cache drive metadata for all mounted file system drives.

### Example
```powershell
Get-OSDCoreCacheDrive -Include C,D -Exclude D
```

Returns OSDCloud cache drive metadata only for drive C.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

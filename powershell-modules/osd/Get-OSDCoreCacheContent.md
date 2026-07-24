# Get-OSDCoreCacheContent

Returns cached OSDCloud content found on local file system drives.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Enumerates mounted file system drives and discovers OSDCloud cache content.
Returns objects with Type, Name, FullName, SizeMB,
DriveRoot, VolumeLabel, VolumeUniqueId, and USB properties.
Exports the returned object to $env:Temp\OSDCoreCacheContent.xml each time the function runs.

If Type is omitted, retur ns all supported cache content types.

Type values:
- ESD: All .esd files under '\<DriveLetter\>:\OSDCloud\OS' recursively.
- ISO: All .iso files under '\<DriveLetter\>:\OSDCloud\ISO' recursively.
- DriverPacks: All .cab, .exe, .msi, and .zip files under
  '\<DriveLetter\>:\OSDCloud\DriverPacks' recursively.
- Drivers: Immediate folders under '\<DriveLetter\>:\OSDCloud\Drivers' that
  contain at least one .inf file in any child folder.
        - Profiles: Immediate folders under '\<DriveLetter\>:\OSDCloud\Profiles'.
- WIM: All .wim files under '\<DriveLetter\>:\OSDCloud\WIM' recursively.
- *: Includes all supported Type values.

## Syntax

```powershell
Get-OSDCoreCacheContent [[-Type] <String[]>] [[-Include] <String[]>] [[-Exclude] <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Type` | `String[]` | False | Optional cache content selector. Supports one or more values. Use '*' to return all supported cache content types. |
| `-Include` | `String[]` | False | Optional list of drive letters to include when searching for OSDCloud cache content. Accepts values such as 'C', 'D:', or 'E:\'. When omitted, all mounted file system drive letters are considered. |
| `-Exclude` | `String[]` | False | Optional list of drive letters to exclude when searching for OSDCloud cache content. Accepts values such as 'C', 'D:', or 'E:\'. Excluded drives are skipped even when they are also present in Include. |

## Examples

### Example
```powershell
Get-OSDCoreCacheContent
```

Returns all supported cache content types.

### Example
```powershell
Get-OSDCoreCacheContent -Type ESD
```

Returns all .esd files under each discovered cache OS folder.

### Example
```powershell
Get-OSDCoreCacheContent -Type ESD,DriverPacks
```

Returns all .esd files and driver pack files from each discovered cache.

### Example
```powershell
Get-OSDCoreCacheContent -Type *
```

Returns all supported cache content types.

### Example
```powershell
Get-OSDCoreCacheContent -Include C,D -Exclude D
```

Searches only drive C for supported cache content types.

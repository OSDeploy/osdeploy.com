# Get-OSDCoreCacheUSBPath

Returns OSDCloud cache paths located on USB drives.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses Get-OSDCoreCacheDrive to enumerate OSDCloud cache drives and returns
the OSDCloud directory path for each discovered cache drive where USB is true,
the file system is NTFS or exFAT, and more than the specified free space is available.

## Syntax

```powershell
Get-OSDCoreCacheUSBPath [[-Include] <String[]>] [[-Exclude] <String[]>] [[-SizeRemaining] <Int32>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Include` | `String[]` | False | Optional list of drive letters to include when searching for OSDCloud cache drives. Accepts values such as 'C', 'D:', or 'E:\'. When omitted, all mounted file system drive letters are considered. |
| `-Exclude` | `String[]` | False | Optional list of drive letters to exclude when searching for OSDCloud cache drives. Accepts values such as 'C', 'D:', or 'E:\'. Excluded drives are skipped even when they are also present in Include. |
| `-SizeRemaining` | `Int32` | False | Optional minimum free space required on the USB cache drive, in GB. The default is 10 GB. |

## Examples

### Example
```powershell
Get-OSDCoreCacheUSBPath
```

Returns OSDCloud directory paths for all discovered USB cache drives with more than 10 GB free and an NTFS or exFAT file system.

### Example
```powershell
Get-OSDCoreCacheUSBPath -Include D
```

Returns the OSDCloud directory path when drive D contains an OSDCloud cache path, is a USB drive, has more than 10 GB free, and is formatted NTFS or exFAT.

### Example
```powershell
Get-OSDCoreCacheUSBPath -SizeRemaining 20
```

Returns OSDCloud directory paths for discovered USB cache drives with more than 20 GB free and an NTFS or exFAT file system.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

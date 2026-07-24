# Test-OSDCoreCacheUSB

Tests whether any OSDCloud cache drive is a USB drive.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses Get-OSDCoreCacheDrive to enumerate OSDCloud cache drives and returns
true when at least one discovered cache drive has USB set to true.

## Syntax

```powershell
Test-OSDCoreCacheUSB [[-Include] <String[]>] [[-Exclude] <String[]>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Include` | `String[]` | False | Optional list of drive letters to include when searching for OSDCloud cache drives. Accepts values such as 'C', 'D:', or 'E:\'. When omitted, all mounted file system drive letters are considered. |
| `-Exclude` | `String[]` | False | Optional list of drive letters to exclude when searching for OSDCloud cache drives. Accepts values such as 'C', 'D:', or 'E:\'. Excluded drives are skipped even when they are also present in Include. |

## Examples

### Example
```powershell
Test-OSDCoreCacheUSB
```

Returns true if any discovered OSDCloud cache drive is a USB drive.

### Example
```powershell
Test-OSDCoreCacheUSB -Include D
```

Returns true if drive D contains an OSDCloud cache path and is a USB drive.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

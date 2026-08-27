# Get-OSDCloudOperatingSystemsIndexes

Returns OSDCloud operating system index entries by architecture.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Reads the cached OSDCloud operating system indexes and returns index
entries for the specified architecture.

## Syntax

```powershell
Get-OSDCloudOperatingSystemsIndexes [-OSArch <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSArch` | `String` | False | Specifies the operating system architecture. Valid values are x64 and ARM64. |

## Examples

### Example
```powershell
Get-OSDCloudOperatingSystemsIndexes
```

Returns x64 operating system index entries from cache.

### Example
```powershell
Get-OSDCloudOperatingSystemsIndexes -OSArch ARM64
```

Returns ARM64 operating system index entries from cache.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

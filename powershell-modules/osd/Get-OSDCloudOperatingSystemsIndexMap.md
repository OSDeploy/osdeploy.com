# Get-OSDCloudOperatingSystemsIndexMap

Returns OSDCloud operating system index map entries by architecture.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Reads the cached OSDCloud operating system index map and returns entries
filtered by the specified architecture.

## Syntax

```powershell
Get-OSDCloudOperatingSystemsIndexMap [-OSArch <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSArch` | `String` | False | Specifies the operating system architecture. Valid values are x64 and ARM64. |

## Examples

### Example
```powershell
Get-OSDCloudOperatingSystemsIndexMap
```

Returns x64 index map entries from cache.

### Example
```powershell
Get-OSDCloudOperatingSystemsIndexMap -OSArch ARM64
```

Returns ARM64 index map entries from cache.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

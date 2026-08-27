# Get-OSDCloudOperatingSystems

Gets OSDCloud operating system entries for a specific architecture.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Queries OSDCloud operating system data and returns entries that match the
requested operating system architecture.

## Syntax

```powershell
Get-OSDCloudOperatingSystems [[-OSArch] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSArch` | `String` | False | Specifies the operating system architecture to return. Valid values: - x64 - amd64 - arm64 |

## Examples

### Example
```powershell
Get-OSDCloudOperatingSystems
```

Returns x64 operating system entries.

### Example
```powershell
Get-OSDCloudOperatingSystems -OSArch arm64
```

Returns ARM64 operating system entries.

### Example
```powershell
Get-OSDCloudOperatingSystems -OSArch amd64
```

Returns x64/amd64 operating system entries.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

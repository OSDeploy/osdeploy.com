# Get-OSDDisk

Gets OSDDisk information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns OSDDisk data for the current system or OSD session context.

## Syntax

```powershell
Get-OSDDisk [[-Number] <UInt32>] [[-BootFromDisk] <Boolean>] [[-IsBoot] <Boolean>] [[-IsReadOnly] <Boolean>]
 [[-IsSystem] <Boolean>] [[-BusType] <String[]>] [[-BusTypeNot] <String[]>] [[-MediaType] <String[]>]
 [[-MediaTypeNot] <String[]>] [[-PartitionStyle] <String[]>] [[-PartitionStyleNot] <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Number` | `UInt32` | False | Specifies the Number to use when running Get-OSDDisk. |
| `-BootFromDisk` | `Boolean` | False | Specifies the BootFromDisk to use when running Get-OSDDisk. |
| `-IsBoot` | `Boolean` | False | Specifies the IsBoot to use when running Get-OSDDisk. |
| `-IsReadOnly` | `Boolean` | False | Specifies the IsReadOnly to use when running Get-OSDDisk. |
| `-IsSystem` | `Boolean` | False | Specifies the IsSystem to use when running Get-OSDDisk. |
| `-BusType` | `String[]` | False | Specifies the BusType to use when running Get-OSDDisk. |
| `-BusTypeNot` | `String[]` | False | Specifies the BusTypeNot to use when running Get-OSDDisk. |
| `-MediaType` | `String[]` | False | Specifies the MediaType to use when running Get-OSDDisk. |
| `-MediaTypeNot` | `String[]` | False | Specifies the MediaTypeNot to use when running Get-OSDDisk. |
| `-PartitionStyle` | `String[]` | False | Specifies the PartitionStyle to use when running Get-OSDDisk. |
| `-PartitionStyleNot` | `String[]` | False | Specifies the PartitionStyleNot to use when running Get-OSDDisk. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-OSDDisk.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

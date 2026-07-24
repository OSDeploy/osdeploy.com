# Get-OSDCoreDeploymentDisk

Retrieves disk objects suitable for OS deployment with enhanced filtering capabilities.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Get-OSDCoreDeploymentDisk queries the system for physical disks and returns disk objects with extended properties including MediaType.
The function automatically filters out offline disks, disks with no media, and incompatible bus types (USB, Virtual, etc.).
It provides comprehensive filtering options based on disk properties such as boot status, bus type, media type, and partition style.

## Syntax

```powershell
Get-OSDCoreDeploymentDisk [[-Number] <UInt32>] [-BootFromDisk] [-IsBoot] [-IsReadOnly] [-IsSystem]
 [[-BusType] <String[]>] [[-BusTypeNot] <String[]>] [[-MediaType] <String[]>] [[-MediaTypeNot] <String[]>]
 [[-PartitionStyle] <String[]>] [[-PartitionStyleNot] <String[]>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Number` | `UInt32` | False | Specifies the disk number to retrieve. Can also be referenced using aliases 'Disk' or 'DiskNumber'. |
| `-BootFromDisk` | `SwitchParameter` | False | Filters disks where the system boots from the disk. |
| `-IsBoot` | `SwitchParameter` | False | Filters disks that contain boot partitions. |
| `-IsReadOnly` | `SwitchParameter` | False | Filters disks based on read-only status. |
| `-IsSystem` | `SwitchParameter` | False | Filters disks that contain system partitions. |
| `-BusType` | `String[]` | False | Filters disks by one or more specific bus types. Valid values: '1394', 'ATA', 'ATAPI', 'Fibre Channel', 'File Backed Virtual', 'iSCSI', 'MMC', 'MAX', 'Microsoft Reserved', 'NVMe', 'RAID', 'SAS', 'SATA', 'SCSI', 'SD', 'SSA', 'Storage Spaces', 'USB', 'Virtual' |
| `-BusTypeNot` | `String[]` | False | Excludes disks with specified bus types. Valid values: '1394', 'ATA', 'ATAPI', 'Fibre Channel', 'File Backed Virtual', 'iSCSI', 'MMC', 'MAX', 'Microsoft Reserved', 'NVMe', 'RAID', 'SAS', 'SATA', 'SCSI', 'SD', 'SSA', 'Storage Spaces', 'USB', 'Virtual' |
| `-MediaType` | `String[]` | False | Filters disks by one or more specific media types. Valid values: 'SSD', 'HDD', 'SCM', 'Unspecified' |
| `-MediaTypeNot` | `String[]` | False | Excludes disks with specified media types. Valid values: 'SSD', 'HDD', 'SCM', 'Unspecified' |
| `-PartitionStyle` | `String[]` | False | Filters disks by one or more specific partition styles. Valid values: 'GPT', 'MBR', 'RAW' |
| `-PartitionStyleNot` | `String[]` | False | Excludes disks with specified partition styles. Valid values: 'GPT', 'MBR', 'RAW' |

## Examples

### Example
```powershell
Get-OSDCoreDeploymentDisk
```

Returns all available deployment-ready disks, excluding USB, virtual, and other incompatible bus types.

### Example
```powershell
Get-OSDCoreDeploymentDisk -Number 0
```

Returns disk 0 if it meets deployment criteria.

### Example
```powershell
Get-OSDCoreDeploymentDisk -MediaType SSD
```

Returns all SSD disks suitable for deployment.

### Example
```powershell
Get-OSDCoreDeploymentDisk -BusType NVMe,SATA -PartitionStyle GPT
```

Returns all NVMe or SATA disks with GPT partition style.

### Example
```powershell
Get-OSDCoreDeploymentDisk -BusTypeNot USB -MediaTypeNot HDD
```

Returns all non-USB, non-HDD disks (typically SSDs and NVMe drives).

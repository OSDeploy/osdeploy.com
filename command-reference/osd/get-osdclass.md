# Get-OSDClass

Returns CimInstance information from common OSD Classes

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns CimInstance information from common OSD Classes

## Syntax

```powershell
Get-OSDClass [[-Class] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Class` | `String` | False | CimInstance Class Name Battery BaseBoard BIOS BootConfiguration ComputerSystem \[DEFAULT\] Desktop DiskPartition DisplayConfiguration Environment LogicalDisk LogicalDiskRootDirectory MemoryArray MemoryDevice NetworkAdapter NetworkAdapterConfiguration OperatingSystem OSRecoveryConfiguration PhysicalMedia PhysicalMemory PnpDevice PnPEntity PortableBattery Processor SCSIController SCSIControllerDevice SMBIOSMemory SystemBIOS SystemEnclosure SystemDesktop SystemPartitions UserDesktop VideoController VideoSettings Volume |

## Examples

### Example
```powershell
OSDClass
Returns CimInstance Win32_ComputerSystem properties
Option 1: Get-OSDClass
Option 2: Get-OSDClass ComputerSystem
Option 3: Get-OSDClass -Class ComputerSystem
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

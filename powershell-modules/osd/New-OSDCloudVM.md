# New-OSDCloudVM

Creates resources by using New-OSDCloudVM.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for New-OSDCloudVM.

## Syntax

```powershell
New-OSDCloudVM [[-CheckpointVM] <Boolean>] [[-Generation] <UInt16>] [[-MemoryStartupGB] <UInt16>]
 [[-NamePrefix] <String>] [[-ProcessorCount] <UInt16>] [[-StartVM] <Boolean>] [[-SwitchName] <String>]
 [[-VHDSizeGB] <UInt16>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-CheckpointVM` | `Boolean` | False | Specifies the value for CheckpointVM. |
| `-Generation` | `UInt16` | False | Specifies the value for Generation. |
| `-MemoryStartupGB` | `UInt16` | False | Specifies the value for MemoryStartupGB. |
| `-NamePrefix` | `String` | False | Specifies the value for NamePrefix. |
| `-ProcessorCount` | `UInt16` | False | Specifies the value for ProcessorCount. |
| `-StartVM` | `Boolean` | False | Specifies the value for StartVM. |
| `-SwitchName` | `String` | False | Specifies the value for SwitchName. |
| `-VHDSizeGB` | `UInt16` | False | Specifies the value for VHDSizeGB. |

## Examples

### Example
```powershell
-Generation <Generation>
Runs New-OSDCloudVM with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

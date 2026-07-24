# New-OSDisk

Creates System | OS | Recovery Partitions for MBR or UEFI Drives in WinPE

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates System | OS | Recovery Partitions for MBR or UEFI Drives in WinPE

## Syntax

```powershell
New-OSDisk [[-Input] <Object>] [[-DiskNumber] <UInt32>] [[-PartitionStyle] <String>] [[-LabelSystem] <String>]
 [[-SizeSystemGpt] <UInt64>] [[-SizeSystemMbr] <UInt64>] [[-SizeMSR] <UInt64>] [[-LabelWindows] <String>]
 [-NoRecoveryPartition] [[-LabelRecovery] <String>] [[-SizeRecovery] <UInt64>] [-Force]
 [-ProgressAction <ActionPreference>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Input` | `Object` | False | No additional description provided. |
| `-DiskNumber` | `UInt32` | False | Specifies the disk number for which to get the associated Disk object Alias = Disk, Number |
| `-PartitionStyle` | `String` | False | Override the automatic Partition Style of the Initialized Disk EFI Default = GPT BIOS Default = MBR Alias = PS |
| `-LabelSystem` | `String` | False | Drive Label of the System Partition Default = System Alias = LS, LabelS |
| `-SizeSystemGpt` | `UInt64` | False | System Partition size for UEFI GPT based Computers Default = 260MB Range = 100MB - 3000MB (3GB) Alias = SSG, Efi, SystemG |
| `-SizeSystemMbr` | `UInt64` | False | System Partition size for BIOS MBR based Computers Default = 260MB Range = 100MB - 3000MB (3GB) Alias = SSM, Mbr, SystemM |
| `-SizeMSR` | `UInt64` | False | MSR Partition size Default = 16MB Range = 16MB - 128MB Alias = MSR |
| `-LabelWindows` | `String` | False | Drive Label of the Windows Partition Default = OS Alias = LW, LabelW |
| `-NoRecoveryPartition` | `SwitchParameter` | False | Alias = SkipRecovery, SkipRecoveryPartition Skips the creation of the Recovery Partition |
| `-LabelRecovery` | `String` | False | Drive Label of the Recovery Partition Default = Recovery Alias = LR, LabelR |
| `-SizeRecovery` | `UInt64` | False | Size of the Recovery Partition Default = 990MB Range = 350MB - 80000MB (80GB) Alias = SR, Recovery |
| `-Force` | `SwitchParameter` | False | Required for execution Alias = F |
| `-WhatIf` | `SwitchParameter` | False | Shows what would happen if the cmdlet runs. The cmdlet is not run. |
| `-Confirm` | `SwitchParameter` | False | Prompts you for confirmation before running the cmdlet. |

## Examples

### Example
```powershell
New-OSDisk
Displays Get-Help New-OSDisk
```

### Example
```powershell
New-OSDisk -Force
Interactive.  Prompted to Confirm Clear-Disk for each Local Disk
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

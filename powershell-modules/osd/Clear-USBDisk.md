# Clear-USBDisk

Clears USBDisk data or state.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Removes existing USBDisk data or configuration and applies the requested reset behavior.

## Syntax

```powershell
Clear-USBDisk [[-Input] <Object>] [[-DiskNumber] <UInt32>] [-Initialize] [[-PartitionStyle] <String>] [-Force]
 [-ShowWarning] [-ProgressAction <ActionPreference>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Input` | `Object` | False | Specifies the Input to use when running Clear-USBDisk. |
| `-DiskNumber` | `UInt32` | False | Specifies the DiskNumber to use when running Clear-USBDisk. |
| `-Initialize` | `SwitchParameter` | False | Specifies the Initialize to use when running Clear-USBDisk. |
| `-PartitionStyle` | `String` | False | Specifies the PartitionStyle to use when running Clear-USBDisk. |
| `-Force` | `SwitchParameter` | False | Specifies the Force to use when running Clear-USBDisk. |
| `-ShowWarning` | `SwitchParameter` | False | Specifies the ShowWarning to use when running Clear-USBDisk. |
| `-WhatIf` | `SwitchParameter` | False | Shows what would happen if the cmdlet runs. The cmdlet is not run. |
| `-Confirm` | `SwitchParameter` | False | Prompts you for confirmation before running the cmdlet. |

## Examples

### Example
```powershell
Demonstrates a common way to run Clear-USBDisk.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

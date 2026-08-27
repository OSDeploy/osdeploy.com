# Clear-LocalDisk

Clears LocalDisk data or state.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Removes existing LocalDisk data or configuration and applies the requested reset behavior.

## Syntax

```powershell
Clear-LocalDisk [[-Input] <Object>] [[-DiskNumber] <UInt32>] [-Initialize] [[-PartitionStyle] <String>]
 [-Force] [-NoResults] [-ShowWarning] [-ProgressAction <ActionPreference>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Input` | `Object` | False | Specifies the Input to use when running Clear-LocalDisk. |
| `-DiskNumber` | `UInt32` | False | Specifies the DiskNumber to use when running Clear-LocalDisk. |
| `-Initialize` | `SwitchParameter` | False | Specifies the Initialize to use when running Clear-LocalDisk. |
| `-PartitionStyle` | `String` | False | Specifies the PartitionStyle to use when running Clear-LocalDisk. |
| `-Force` | `SwitchParameter` | False | Specifies the Force to use when running Clear-LocalDisk. |
| `-NoResults` | `SwitchParameter` | False | Specifies the NoResults to use when running Clear-LocalDisk. |
| `-ShowWarning` | `SwitchParameter` | False | Specifies the ShowWarning to use when running Clear-LocalDisk. |
| `-WhatIf` | `SwitchParameter` | False | Shows what would happen if the cmdlet runs. The cmdlet is not run. |
| `-Confirm` | `SwitchParameter` | False | Prompts you for confirmation before running the cmdlet. |

## Examples

### Example
```powershell
Demonstrates a common way to run Clear-LocalDisk.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

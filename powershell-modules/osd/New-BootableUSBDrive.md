# New-BootableUSBDrive

Creates BootableUSBDrive resources.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Builds new BootableUSBDrive resources based on the provided parameters.

## Syntax

```powershell
New-BootableUSBDrive [[-BootLabel] <String>] [[-DataLabel] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-BootLabel` | `String` | False | Specifies the BootLabel to use when running New-BootableUSBDrive. |
| `-DataLabel` | `String` | False | Specifies the DataLabel to use when running New-BootableUSBDrive. |

## Examples

### Example
```powershell
Demonstrates a common way to run New-BootableUSBDrive.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Invoke-OSDCloudDriverPackMDT

Downloads a matching DriverPack to %OSDisk%\Drivers

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads a matching DriverPack to %OSDisk%\Drivers

## Syntax

```powershell
Invoke-OSDCloudDriverPackMDT [[-Manufacturer] <String>] [[-Product] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Manufacturer` | `String` | False | No additional description provided. |
| `-Product` | `String` | False | No additional description provided. |

## Examples

### Example
```powershell
Invoke-OSDCloudDriverPackMDT
Downloads and stages a matching driver pack during an MDT task sequence.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

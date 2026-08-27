# Save-ZTIDriverPack

Downloads the driver pack for a computer during MDT/ConfigMgr task sequence

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads and stages the matching driver pack from OSDCloud during Lite Touch or Zero Touch deployment.
Requires an active task sequence environment.

## Syntax

```powershell
Save-ZTIDriverPack [[-Manufacturer] <String>] [[-Product] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Manufacturer` | `String` | False | Computer manufacturer. Default is auto-detected |
| `-Product` | `String` | False | Computer product model. Default is auto-detected |

## Examples

### Example
```powershell
Save-ZTIDriverPack
Downloads the driver pack for the current computer during task sequence
```

### Example
```powershell
Save-ZTIDriverPack -Manufacturer 'Dell' -Product 'Latitude 5420'
Downloads the driver pack for a specific Dell model
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

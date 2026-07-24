# Get-MyDriverPack

Retrieves the driver pack for the current computer from OSDCloud

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Queries OSDCloud for a matching driver pack based on computer manufacturer and product model.

## Syntax

```powershell
Get-MyDriverPack [[-Manufacturer] <String>] [[-Product] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Manufacturer` | `String` | False | Computer manufacturer. Default is auto-detected from current system |
| `-Product` | `String` | False | Computer product model. Default is auto-detected from current system |

## Examples

### Example
```powershell
Get-MyDriverPack
Returns the driver pack for the current computer
```

### Example
```powershell
Get-MyDriverPack -Manufacturer 'Lenovo' -Product 'ThinkPad X1'
Returns the driver pack for the specified model
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

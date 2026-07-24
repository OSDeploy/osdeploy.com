# Start-OSDCloudGUIDev

OSDCloud imaging using the command line

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

OSDCloud imaging using the command line

## Syntax

```powershell
Start-OSDCloudGUIDev [[-BrandName] <String>] [[-BrandColor] <String>] [[-ComputerManufacturer] <String>]
 [[-ComputerProduct] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-BrandName` | `String` | False | Sets the GUI brand text shown in the header/title. |
| `-BrandColor` | `String` | False | Sets the GUI brand color. |
| `-ComputerManufacturer` | `String` | False | Overrides detected manufacturer for driver pack selection. |
| `-ComputerProduct` | `String` | False | Overrides detected product/system identifier for driver pack selection. |

## Examples

### Example
```powershell
Start-OSDCloudGUIDev
Starts the OSDCloud development GUI workflow.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

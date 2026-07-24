# Start-OSDCloudGUI

OSDCloud imaging using the command line

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

OSDCloud imaging using the command line

## Syntax

```powershell
Start-OSDCloudGUI [[-BrandName] <String>] [[-BrandColor] <String>] [[-ComputerManufacturer] <String>]
 [[-ComputerProduct] <String>] [-v2] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-BrandName` | `String` | False | Sets the GUI brand text shown in the header/title. |
| `-BrandColor` | `String` | False | Sets the GUI brand color. |
| `-ComputerManufacturer` | `String` | False | Overrides detected manufacturer for driver pack filtering. |
| `-ComputerProduct` | `String` | False | Overrides detected product/system identifier for driver pack filtering. |
| `-v2` | `SwitchParameter` | False | Legacy compatibility switch for manufacturer-based driver pack filtering. |

## Examples

### Example
```powershell
Start-OSDCloudGUI
Starts OSDCloud GUI with detected device values.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

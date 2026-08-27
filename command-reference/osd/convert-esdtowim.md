# Convert-EsdToWim

Converts an ESD file into a WIM image.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Exports non-setup Windows indexes from an ESD source into a new WIM file.

## Syntax

```powershell
Convert-EsdToWim [-esdFullName] <String> [[-wimFullName] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-esdFullName` | `String` | True | Full path to the source ESD file. |
| `-wimFullName` | `String` | False | Destination WIM file path. If omitted, a WIM is created beside the ESD. |

## Examples

### Example
```powershell
Convert-EsdToWim -esdFullName 'C:\Media\install.esd'
Exports Windows image indexes from the ESD into install.wim.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

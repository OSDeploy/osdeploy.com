# Get-MyBiosUpdate

Gets MyBiosUpdate information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns MyBiosUpdate data for the current system or OSD session context.

## Syntax

```powershell
Get-MyBiosUpdate [[-Manufacturer] <String>] [[-Product] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Manufacturer` | `String` | False | Specifies the Manufacturer to use when running Get-MyBiosUpdate. |
| `-Product` | `String` | False | Specifies the Product to use when running Get-MyBiosUpdate. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-MyBiosUpdate.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

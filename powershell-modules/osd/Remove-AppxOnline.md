# Remove-AppxOnline

Removes AppxOnline items.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Deletes matching AppxOnline items from the current context.

## Syntax

```powershell
Remove-AppxOnline [-GridRemoveAppx] [-GridRemoveAppxPP] [[-Name] <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-GridRemoveAppx` | `SwitchParameter` | False | Specifies the GridRemoveAppx to use when running Remove-AppxOnline. |
| `-GridRemoveAppxPP` | `SwitchParameter` | False | Specifies the GridRemoveAppxPP to use when running Remove-AppxOnline. |
| `-Name` | `String[]` | False | Specifies the Name to use when running Remove-AppxOnline. |

## Examples

### Example
```powershell
Demonstrates a common way to run Remove-AppxOnline.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

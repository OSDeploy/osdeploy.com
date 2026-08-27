# Get-AzClipboard

Read a secret value from the Azure clipboard Key Vault.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Connects to Azure if needed, finds the first Key Vault tagged with AzClipboard, and returns
the named secret as plain text.

## Syntax

```powershell
Get-AzClipboard [[-Name] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Name` | `String` | False | The name of the Key Vault secret to read. The default secret name is Clipboard. |

## Examples

### Example
```powershell
Get-AzClipboard
Returns the value stored in the default Clipboard secret.
```

### Example
```powershell
Get-AzClipboard -Name Clipboard
Returns the value stored in the Clipboard secret explicitly.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Get-AzClipboard.md](https://github.com/OSDeploy/OSD/blob/master/docs/Get-AzClipboard.md)

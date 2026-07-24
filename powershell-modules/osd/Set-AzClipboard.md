# Set-AzClipboard

Write the current clipboard text to the Azure clipboard Key Vault.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Connects to Azure if needed, finds the first Key Vault tagged with AzClipboard, and stores
the current clipboard text in the named secret as plain text.

## Syntax

```powershell
Set-AzClipboard [[-Name] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Name` | `String` | False | The name of the Key Vault secret to write. The default secret name is Clipboard. |

## Examples

### Example
```powershell
Set-AzClipboard
Copies the current clipboard text into the default Clipboard secret.
```

### Example
```powershell
Set-AzClipboard -Name Clipboard
Copies the current clipboard text into the Clipboard secret explicitly.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Set-AzClipboard.md](https://github.com/OSDeploy/OSD/blob/master/docs/Set-AzClipboard.md)

# Get-CloudSecret

Read a secret from Azure Key Vault.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Connects to Azure if needed and returns the named Key Vault secret as plain text.

## Syntax

```powershell
Get-CloudSecret [-VaultName] <String> [-Name] <String> [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-VaultName` | `String` | True | Name of the Key Vault that contains the secret. |
| `-Name` | `String` | True | Name of the secret to read. |

## Examples

### Example
```powershell
Get-CloudSecret -VaultName contoso -Name Script
Returns the secret text from the specified vault.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

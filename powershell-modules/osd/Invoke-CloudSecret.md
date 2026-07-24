# Invoke-CloudSecret

Invoke a secret retrieved from Azure Key Vault.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Loads the named secret with Get-CloudSecret and either invokes it directly, writes it to a
temporary file, or runs it elevated depending on the selected invoke mode.

## Syntax

```powershell
Invoke-CloudSecret [-VaultName] <String> [-Name] <String> [-Invoke <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-VaultName` | `String` | True | Name of the Key Vault that contains the secret. |
| `-Name` | `String` | True | Name of the secret to read. |
| `-Invoke` | `String` | False | Choose how to run the secret content: Command, File, or FileRunas. |

## Examples

### Example
```powershell
Invoke-CloudSecret -VaultName contoso -Name Script
Invokes the retrieved secret in the current session.
```

### Example
```powershell
Invoke-CloudSecret -VaultName contoso -Name Script -Invoke FileRunas
Writes the secret to a temporary file and runs it elevated.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# ConvertTo-PSKeyVaultSecret

Converts a value to an Azure Key Vault Secret

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Converts a value to an Azure Key Vault Secret

## Syntax

### FromUriContent (Default)
```powershell
ConvertTo-PSKeyVaultSecret -VaultName <String> -Name <String> -Uri <Uri> [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### FromClipboard
```powershell
ConvertTo-PSKeyVaultSecret -VaultName <String> -Name <String> [-Clipboard] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### FromFile
```powershell
ConvertTo-PSKeyVaultSecret -VaultName <String> -Name <String> -File <FileInfo>
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### FromString
```powershell
ConvertTo-PSKeyVaultSecret -VaultName <String> -Name <String> -String <String>
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-VaultName` | `String` | True | Specifies the name of the key vault to which the secret belongs. This cmdlet constructs the fully qualified domain name (FQDN) of a key vault based on the name that this parameter specifies and your current environment. |
| `-Name` | `String` | True | Specifies the name of the secret to set |
| `-Uri` | `Uri` | True | Uri content to set as the Azure Key Vault secret |
| `-Clipboard` | `SwitchParameter` | True | Clipboard raw text to set as the Azure Key Vault secret |
| `-File` | `FileInfo` | True | File content to set as the Azure Key Vault secret |
| `-String` | `String` | True | String to set as the Azure Key Vault secret |

## Examples

No examples provided in source documentation.

## Related

* [https://osd.osdeploy.com](https://osd.osdeploy.com)

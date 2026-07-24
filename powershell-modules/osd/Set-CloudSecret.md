# Set-CloudSecret

Convert content to an Azure Key Vault secret.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Reads content from a URL, the clipboard, a file, or a raw string and stores it in Azure Key
Vault as a secret.

## Syntax

### FromUriContent (Default)
```powershell
Set-CloudSecret [-VaultName] <String> [-Name] <String> -Uri <Uri> [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### FromClipboard
```powershell
Set-CloudSecret [-VaultName] <String> [-Name] <String> [-Clipboard] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### FromFile
```powershell
Set-CloudSecret [-VaultName] <String> [-Name] <String> -File <FileInfo> [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### FromString
```powershell
Set-CloudSecret [-VaultName] <String> [-Name] <String> -String <String> [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-VaultName` | `String` | True | Name of the Key Vault that receives the secret. |
| `-Name` | `String` | True | Name of the secret to set. |
| `-Uri` | `Uri` | True | URI content to set as the Azure Key Vault secret. |
| `-Clipboard` | `SwitchParameter` | True | Clipboard text to set as the Azure Key Vault secret. |
| `-File` | `FileInfo` | True | File content to set as the Azure Key Vault secret. |
| `-String` | `String` | True | String content to set as the Azure Key Vault secret. |

## Examples

### Example
```powershell
Set-CloudSecret -VaultName contoso -Name Script -File .\script.ps1
Uploads file contents to Key Vault.
```

### Example
```powershell
Set-CloudSecret -VaultName contoso -Name Script -Clipboard
Stores clipboard contents in Key Vault.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

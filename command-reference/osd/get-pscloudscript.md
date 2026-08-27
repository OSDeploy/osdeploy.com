# Get-PSCloudScript

Development function to get the contents of a PSCloudScript.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Development function to get the contents of a PSCloudScript.
Optionally allows for execution by command or file.

## Syntax

### FromUriContent (Default)
```powershell
Get-PSCloudScript [-Uri] <String> [-Invoke <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### FromAzKeyVaultSecret
```powershell
Get-PSCloudScript -VaultName <String> [-Name <String[]>] [-Invoke <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### FromClipboard
```powershell
Get-PSCloudScript [-Clipboard] [-Invoke <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### FromFile
```powershell
Get-PSCloudScript -File <FileInfo> [-Invoke <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### FromString
```powershell
Get-PSCloudScript -String <String> [-Invoke <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### FromGitHubRepo
```powershell
Get-PSCloudScript -RepoOwner <String> -RepoName <String> [-GithubPath <String>] [-Invoke <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `String` | True | Uri content to use as a PSCloudScript |
| `-VaultName` | `String` | True | Specifies the name of the key vault to which the secret belongs. This cmdlet constructs the fully qualified domain name (FQDN) of a key vault based on the name that this parameter specifies and your current environment. |
| `-Name` | `String[]` | False | Specifies the name of the secret to get the content to use as a PSCloudScript |
| `-Clipboard` | `SwitchParameter` | True | Clipboard raw text to use as a PSCloudScript |
| `-File` | `FileInfo` | True | File content to use as a PSCloudScript |
| `-String` | `String` | True | String to use as a PSCloudScript |
| `-RepoOwner` | `String` | True | GitHub Organization |
| `-RepoName` | `String` | True | GitHub Repo |
| `-GithubPath` | `String` | False | GitHub Path |
| `-Invoke` | `String` | False | No additional description provided. |

## Examples

### Example
```powershell
Get-PSCloudScript -Uri 'https://example.com/script.ps1'
```

## Related

* [https://osd.osdeploy.com](https://osd.osdeploy.com)

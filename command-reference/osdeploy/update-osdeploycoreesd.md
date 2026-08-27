# Update-OSDeployCoreESD

Downloads Windows Enterprise ESD files from the latest OSDeploy OS catalog.

| Property | Value                                                   |
|----------|---------------------------------------------------------|
| Module   | OSDeploy                                                |
| Platform | Windows 11 (amd64 / arm64)                             |
| Requires | PowerShell 7.6, internet access                         |

## Description

Locates the most recent XML catalog file in the OSDeploy operating system catalogs directory, resolves the en-US Enterprise x64 and ARM64 ESD entries, and downloads them to `C:\ProgramData\OSDeployCore\cache\windows-esd`.

Each file is skipped silently when it is already present and its SHA256 checksum matches the catalog value. For files that need downloading, the URL is first tested for reachability. All pending downloads are confirmed with the user before any transfer begins.

SHA256 verification is performed after every download. A terminating error is thrown when the checksum does not match or when `curl.exe` fails.

{% hint style="info" %}
`Update-OSDeployCoreESD` is called automatically by `Update-OSDeployCoreOS` and `Update-OSDeployCore`. Run it directly only when you want to cache ESD files without importing them as OS sources.
{% endhint %}

## Syntax

```powershell
Update-OSDeployCoreESD [-Force] [[-Architecture] <String>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter        | Type     | Required | Description                                                                                             |
|------------------|----------|----------|---------------------------------------------------------------------------------------------------------|
| `-Force`         | `Switch` | No       | Re-downloads each ESD file even when it already exists in the cache with a matching SHA256 checksum.    |
| `-Architecture`  | `String` | No       | Limits downloads to a single architecture: `amd64` or `arm64`. When omitted, both architectures are downloaded. |
| `-WhatIf`        | `Switch` | No       | Shows which ESD files would be downloaded without performing any transfers.                             |
| `-Confirm`       | `Switch` | No       | Prompts for confirmation before each download.                                                          |

## Examples

```powershell
# Test reachability, confirm, then download any missing ESD files
Update-OSDeployCoreESD
```

```powershell
# Re-download both ESD files regardless of whether they are already cached
Update-OSDeployCoreESD -Force
```

```powershell
# Preview which ESD files would be downloaded without performing any work
Update-OSDeployCoreESD -WhatIf
```

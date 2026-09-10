# Update-OSDeployCoreESD

Downloads Windows Enterprise ESD files from the latest OSDeploy OS catalog.

| Property | Value                                                   |
|----------|---------------------------------------------------------|
| Module   | OSDeploy                                                |
| Platform | Windows 11 (amd64 / arm64)                             |
| Requires | PowerShell 7.6, Administrator rights, `curl.exe`, internet access, valid license |
| Output   | `System.IO.FileInfo[]` for verified usable ESD files    |

## Description

Locates the newest XML catalog in the OSDeploy module, resolves eligible en-US Enterprise ESD entries, and downloads them to the release folder under `C:\ProgramData\OSDeployCore\OSDCloud\OS`, such as `Windows 11 25H2`.

Each current file is reused when its SHA256 checksum matches. A verified older-catalog file can be retained after an upgrade prompt. Pending URLs are tested before per-file confirmations and transfers begin.

SHA256 verification follows every download. Failed transfers and checksums use bounded automatic retry followed by an interactive retry decision and cleanup options.

{% hint style="info" %}
`Update-OSDeployCoreESD` is called automatically by `Update-OSDeployCore`. Run it directly when only the ESD cache needs updating, then use `Update-OSDeployCoreRE` to export recovery content.
{% endhint %}

## Syntax

```powershell
Update-OSDeployCoreESD [-Force] [[-Architecture] <String>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter        | Type     | Required | Description                                                                                             |
|------------------|----------|----------|---------------------------------------------------------------------------------------------------------|
| `-Force`         | `Switch` | No       | Bypass verified cache reuse and request fresh downloads. It does not bypass `ShouldContinue` prompts. |
| `-Architecture`  | `String` | No       | `amd64` or `arm64`. AMD64 hosts consider x64 then ARM64 by default; ARM64 hosts consider ARM64 only. |
| `-WhatIf`        | `Switch` | No       | Suppress gated directory, deletion, download, retry, and cleanup operations. Initialization, hashing, URL tests, and Yes/No prompts can still occur. |
| `-Confirm`       | `Switch` | No       | Add confirmation at `ShouldProcess` boundaries; independent `ShouldContinue` prompts still appear. |

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

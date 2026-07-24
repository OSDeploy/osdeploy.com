# Get-GithubRawContent

Retrieves content from GitHub or Gist raw URLs.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves one or more GitHub/Gist URLs to raw content URLs and retrieves the
content for each URL using Invoke-RestMethod.
Failed URLs emit warnings while
successful responses continue to stream to the pipeline.

## Syntax

```powershell
Get-GithubRawContent [-Uri] <Uri[]> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `Uri[]` | True | A GitHub, Gist, raw URL, or other absolute URI to retrieve content from. |

## Examples

### Example
```powershell
Get-GithubRawContent -Uri 'https://github.com/OSDeploy/OSD/blob/master/README.md'
Retrieves the raw README.md content.
```

### Example
```powershell
'https://gist.github.com/user/0123456789abcdef' | Get-GithubRawContent
Retrieves content for each file in the gist.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-GithubRawUrl

Resolves a GitHub or Gist URL to one or more raw content URLs.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Converts common GitHub URL forms (blob, raw, and gist) to direct raw content
URLs that can be consumed by download or content retrieval commands.
For gist
pages, the function queries the GitHub Gist API to return raw URLs for all files.

## Syntax

```powershell
Get-GithubRawUrl [-Uri] <Uri[]> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `Uri[]` | True | A GitHub, Gist, raw URL, or other absolute URI to resolve. |

## Examples

### Example
```powershell
Get-GithubRawUrl -Uri 'https://github.com/OSDeploy/OSD/blob/master/README.md'
Returns the matching raw.githubusercontent.com URL for README.md.
```

### Example
```powershell
Get-GithubRawUrl -Uri 'https://gist.github.com/user/0123456789abcdef'
Returns raw URLs for files in the specified gist.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

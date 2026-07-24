# Get-OSDPad

Gets information returned by Get-OSDPad.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for Get-OSDPad.

## Syntax

### Standalone (Default)
```powershell
Get-OSDPad [-Brand <String>] [-Color <String>] [-Hide <String[]>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

### GitHub
```powershell
Get-OSDPad [-RepoOwner] <String> [-RepoName] <String> [[-RepoFolder] <String>] [-OAuth <String>]
 [-Brand <String>] [-Color <String>] [-Hide <String[]>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-RepoOwner` | `String` | True | Specifies the value for RepoOwner. |
| `-RepoName` | `String` | True | Specifies the value for RepoName. |
| `-RepoFolder` | `String` | False | Specifies the value for RepoFolder. |
| `-OAuth` | `String` | False | Specifies the value for OAuth. |
| `-Brand` | `String` | False | Specifies the value for Brand. |
| `-Color` | `String` | False | Specifies the value for Color. |
| `-Hide` | `String[]` | False | Specifies the value for Hide. |

## Examples

### Example
```powershell
-RepoName <RepoName>
Runs Get-OSDPad with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

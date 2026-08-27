# Start-OSDPadCategories

Starts the workflow for Start-OSDPadCategories.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for Start-OSDPadCategories.

## Syntax

```powershell
Start-OSDPadCategories [-RepoOwner] <String> [-RepoName] <String> [-OAuth <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-RepoOwner` | `String` | True | Specifies the value for RepoOwner. |
| `-RepoName` | `String` | True | Specifies the value for RepoName. |
| `-OAuth` | `String` | False | Specifies the value for OAuth. |

## Examples

### Example
```powershell
-RepoName <RepoName>
Runs Start-OSDPadCategories with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

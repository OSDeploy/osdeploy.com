# Start-OSDeployPad

Starts the workflow for Start-OSDeployPad.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for Start-OSDeployPad.

## Syntax

```powershell
Start-OSDeployPad [[-RepoFolder] <String>] [-OAuth <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-RepoFolder` | `String` | False | Specifies the value for RepoFolder. |
| `-OAuth` | `String` | False | Specifies the value for OAuth. |

## Examples

### Example
```powershell
-OAuth <OAuth>
Runs Start-OSDeployPad with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

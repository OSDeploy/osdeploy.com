# Get-OSDHelp

Gets OSDHelp information.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns OSDHelp data for the current system or OSD session context.

## Syntax

```powershell
Get-OSDHelp [[-RepoFolder] <String>] [-OAuth <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-RepoFolder` | `String` | False | Specifies the RepoFolder to use when running Get-OSDHelp. |
| `-OAuth` | `String` | False | Specifies the OAuth to use when running Get-OSDHelp. |

## Examples

### Example
```powershell
Demonstrates a common way to run Get-OSDHelp.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

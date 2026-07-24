# Get-AzOSDTechId

Find Azure AD users for an OSD tech identifier prefix.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Connects to Azure with device authentication, selects a subscription when multiple subscriptions
are available, and returns Azure AD users whose name starts with the supplied value.

## Syntax

```powershell
Get-AzOSDTechId [-AzureAdUserName] <String> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-AzureAdUserName` | `String` | True | Prefix to search for in Azure AD user names. |

## Examples

### Example
```powershell
Get-AzOSDTechId -AzureAdUserName alex
Finds Azure AD users whose names start with alex.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Get-AzOSDTechId.md](https://github.com/OSDeploy/OSD/blob/master/docs/Get-AzOSDTechId.md)

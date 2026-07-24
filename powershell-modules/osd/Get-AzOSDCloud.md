# Get-AzOSDCloud

Initialize the local OSDCloud Azure workspace.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates the local OSDCloud folder structure under C:\OSDCloud, copies the repository's Bicep
and Terraform templates into place, and optionally opens the workspace in Visual Studio Code.

## Syntax

```powershell
Get-AzOSDCloud [-edit] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-edit` | `SwitchParameter` | False | Open the C:\OSDCloud workspace in Visual Studio Code after the files are copied. |

## Examples

### Example
```powershell
Get-AzOSDCloud
Creates the local workspace and copies the Azure IaC templates.
```

### Example
```powershell
Get-AzOSDCloud -edit
Creates the local workspace and opens it in Visual Studio Code.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Get-AzOSDCloud.md](https://github.com/OSDeploy/OSD/blob/master/docs/Get-AzOSDCloud.md)

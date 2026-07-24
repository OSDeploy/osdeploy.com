# Start-OSDCloudAzure

Start an OSDCloud deployment from Azure Storage.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Runs from WinPE, installs the OSDCloudAzure dependencies, connects to Azure, discovers
available OSDCloud resources, and starts the deployment workflow when an image is available.

## Syntax

```powershell
Start-OSDCloudAzure [-Force] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Force` | `SwitchParameter` | False | Reset OSDCloudAzure state before continuing. |

## Examples

### Example
```powershell
Start-OSDCloudAzure
Starts an Azure-backed OSDCloud deployment using the current selection.
```

### Example
```powershell
Start-OSDCloudAzure -Force
Resets the current Azure image selection and restarts the deployment flow.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Start-OSDCloudAzure.md](https://github.com/OSDeploy/OSD/blob/master/docs/Start-OSDCloudAzure.md)

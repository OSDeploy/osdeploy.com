# Connect-OSDCloudAzure

Connect to Azure and initialize OSDCloudAzure session state.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Installs the Azure and Microsoft Graph modules required by OSDCloudAzure, signs in to Azure,
optionally prompts for a subscription when multiple subscriptions are available, and populates
the global context, token, and header variables used by the Azure deployment workflow.

## Syntax

```powershell
Connect-OSDCloudAzure [-UseDeviceAuthentication] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-UseDeviceAuthentication` | `SwitchParameter` | False | Use device-code authentication instead of the interactive Azure sign-in flow. |

## Examples

### Example
```powershell
Connect-OSDCloudAzure
Signs in to Azure using the interactive browser-based authentication flow.
```

### Example
```powershell
Connect-OSDCloudAzure -UseDeviceAuthentication
Signs in to Azure by using device-code authentication.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://github.com/OSDeploy/OSD/blob/master/docs/Connect-OSDCloudAzure.md](https://github.com/OSDeploy/OSD/blob/master/docs/Connect-OSDCloudAzure.md)

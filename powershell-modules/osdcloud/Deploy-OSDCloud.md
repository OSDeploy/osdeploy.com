# Deploy-OSDCloud

Starts an OSDCloud operating system deployment.

| Property | Value                                                                         |
|----------|-------------------------------------------------------------------------------|
| Module   | OSDCloud                                                                      |
| Platform | WinPE (amd64 / arm64)                                                         |
| Requires | OSDCloud module                                                               |

## Description

Initializes and runs an OSDCloud deployment workflow. By default, launches the graphical UI (UX) so the operator can configure deployment settings before starting. Use `-CLI` to skip the UI and run the workflow immediately in the current console session.

OSDCloud collects anonymous analytic data about the deployment environment and system configuration to help improve the product. No personally identifiable information (PII) is collected. By using OSDCloud you consent to this collection as described in the [privacy policy](https://github.com/OSDeploy/OSDCloud/blob/main/PRIVACY.md).

## Syntax

```powershell
Deploy-OSDCloud [[-WorkflowName] <String>] [-CLI] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WorkflowName` | `String` | No | The name of the OSDCloud workflow to run. Default: `default`. Alias: `Name`. Available workflows are in the module's `workflow\` folder. |
| `-CLI` | `Switch` | No | Skips the graphical UX and runs the deployment workflow immediately in the current console session. |

## Examples

```powershell
# Launch the graphical UX for the default workflow
Deploy-OSDCloud
```

```powershell
# Run the default workflow immediately without the UX
Deploy-OSDCloud -CLI
```

```powershell
# Launch the graphical UX for the 'latest' workflow
Deploy-OSDCloud -WorkflowName 'latest'
```

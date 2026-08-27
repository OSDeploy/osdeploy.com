# Install-AzOSDIacTools

Install prerequisite IaC tooling for OSDCloud Azure.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Detects Terraform, Bicep, and Azure CLI, installs missing components, updates the current
user's PATH, and verifies the OSD PowerShell modules needed by the Azure IaC workflow.

## Syntax

```powershell
Install-AzOSDIacTools [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Install-AzOSDIacTools
Installs any missing tooling and validates the OSD modules.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

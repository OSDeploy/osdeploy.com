# Get-ReAgentXml

Returns information from the Windows Recovery Agent XML file

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Reads and parses the Reagent.xml file to extract Windows Recovery Environment configuration and status information.
This function must be run in Windows.

## Syntax

```powershell
Get-ReAgentXml [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-ReAgentXml
Returns ReAgent.xml configuration details
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

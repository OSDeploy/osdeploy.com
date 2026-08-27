# Initialize-OSDCoreDevice

Collects local hardware, firmware, TPM, and network details for OSDCloud.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Initialize-OSDCoreDevice gathers device information from CIM classes, firmware,
and environment data, then normalizes manufacturer/model/product values for
workflow use.
It writes diagnostic logs to $env:TEMP\osdcloud-logs, attempts to
copy logs to an available OSDCloudLogs path, and populates
$global:OSDCoreDevice with an ordered property set used by downstream OSDCloud
deployment logic.

## Syntax

```powershell
Initialize-OSDCoreDevice [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Initialize-OSDCoreDevice
```

Collects current device metadata, creates or updates
$global:OSDCoreDevice, and writes log artifacts for troubleshooting.

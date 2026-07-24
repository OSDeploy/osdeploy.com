# Invoke-WebPSScript

Executes a PowerShell script from a URL.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads and executes a PowerShell script from a URL.

## Syntax

```powershell
Invoke-WebPSScript [-Uri] <Uri> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `Uri` | True | The URL of the PowerShell script to execute. Redirects are not allowed. |

## Examples

### Example
```powershell
Invoke-WebPSScript -Uri 'https://example.com/script.ps1'
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

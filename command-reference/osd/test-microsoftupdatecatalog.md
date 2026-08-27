# Test-MicrosoftUpdateCatalog

Tests connectivity to Microsoft Update Catalog.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Sends an HTTP request to Microsoft Update Catalog and returns True when the
endpoint is reachable with a successful or redirect status code.
Uses a
HEAD request first, then falls back to GET if needed.

## Syntax

```powershell
Test-MicrosoftUpdateCatalog [[-Uri] <String>] [[-TimeoutSec] <Int32>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `String` | False | The Microsoft Update Catalog endpoint to test. |
| `-TimeoutSec` | `Int32` | False | Timeout in seconds for each HTTP request attempt. |

## Examples

### Example
```powershell
Test-MicrosoftUpdateCatalog
Returns True when the default Microsoft Update Catalog endpoint is reachable.
```

### Example
```powershell
Test-MicrosoftUpdateCatalog -TimeoutSec 5
Tests connectivity with a shorter timeout.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

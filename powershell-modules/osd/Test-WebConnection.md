# Test-WebConnection

Tests web connectivity to a target URI using a live TCP connection and HTTP HEAD request.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Opens a live TCP connection and sends HTTP HEAD requests to the specified
URI, returning $true when the request succeeds and $false otherwise.
If a URI
is provided without a scheme, both https:// and http:// are tested.

## Syntax

```powershell
Test-WebConnection [[-Uri] <Uri>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `Uri` | False | URI to test. Values from the pipeline are supported. |

## Examples

### Example
```powershell
Test-WebConnection -Uri 'http://example.com'
Returns $true when the target responds to an HTTP HEAD request.
```

### Example
```powershell
'google.com' | Test-WebConnection
Tests a bare URI supplied from the pipeline by checking both HTTPS and HTTP.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

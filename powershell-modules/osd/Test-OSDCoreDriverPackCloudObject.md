# Test-OSDCoreDriverPackCloudObject

Tests whether an OSDCore driver pack object URL is reachable.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Reads the Url property from the supplied driver pack object, or from
$global:OSDCoreDriverPackCloudObject when no object is supplied, and returns
$true when a live TCP connection and HTTP HEAD request can reach it.
Returns $false when the object
is missing, the Url property is empty, or the URL test fails.
HTTP and HTTPS
are both tested for host-only web URLs so systems with an invalid date can still
detect basic network reachability over HTTP.
Specific absolute file URLs are
tested exactly as supplied.

## Syntax

```powershell
Test-OSDCoreDriverPackCloudObject [[-DriverPackCloudObject] <PSObject>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DriverPackCloudObject` | `PSObject` | False | Driver pack object containing a Url property to test. |

## Examples

### Example
```powershell
Test-OSDCoreDriverPackCloudObject
Tests the Url property on $global:OSDCoreDriverPackCloudObject.
```

### Example
```powershell
Test-OSDCoreDriverPackCloudObject -DriverPackCloudObject $global:OSDCoreDriverPackCloudObject
Tests the Url property on the supplied driver pack object.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

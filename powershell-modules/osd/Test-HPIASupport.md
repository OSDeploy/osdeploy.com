# Test-HPIASupport

Tests whether the current HP platform is supported by HPIA.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads the HP platform catalog, reads the platform IDs from the XML, and
compares the local baseboard product ID to determine whether HPIA support is
available on this device.

## Syntax

```powershell
Test-HPIASupport
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Test-HPIASupport
Returns True when the current device platform is listed in the HPIA platform catalog.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

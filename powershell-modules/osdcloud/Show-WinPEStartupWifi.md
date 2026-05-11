# Show-WinPEStartupWifi

Establishes and validates Wi-Fi connectivity in WinPE.

| Property | Value                                                              |
|----------|--------------------------------------------------------------------|
| Module   | OSDCloud                                                           |
| Platform | WinPE only                                                         |
| Requires | OSDCloud module, WinPE environment, Wi-Fi hardware, Wi-Fi DLLs    |

{% hint style="info" %}
Wi-Fi startup is skipped automatically if any of the required DLLs are missing from the WinPE image.
{% endhint %}

## Description

Performs Wi-Fi startup operations for WinPE. If internet access is not detected, validates required wireless components and attempts to connect using configured options or an interactive Wi-Fi flow. Then validates local IP assignment and renews DHCP leases when needed.

The following DLLs must be present in the WinPE image for Wi-Fi to start:

- `dmcmnutils.dll`
- `mdmpostprocessevaluator.dll`
- `mdmregistration.dll`
- `raschap.dll`
- `raschapext.dll`
- `rastls.dll`
- `rastlsext.dll`

If any of these DLLs are missing, the function exits without attempting a connection.

## Syntax

```powershell
Show-WinPEStartupWifi
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Attempt to establish Wi-Fi connectivity and validate network initialization
Show-WinPEStartupWifi
```

# Show-OSDCloudDeviceInfo

Displays comprehensive WinPE device and hardware information during OS deployment startup.

| Property | Value                         |
|----------|-------------------------------|
| Module   | OSDCloud                      |
| Platform | WinPE (amd64 / arm64)         |
| Requires | OSDCloud module               |

## Description

Gathers and displays detailed hardware and environment information in Windows PE, including system specifications, device identifiers, processor details, memory configuration, disk drives, and network adapters. Initializes the OSDCloud device environment and exports hardware WMI data to log files in `$env:TEMP\osdcloud-logs`.

Validates system memory and warns if the device has less than 6 GB RAM.

Information displayed:

- OSDCloud module version
- WinPE version, architecture, and hostname
- System manufacturer, model, and BIOS
- Processor details and memory (GB)
- Disk drives
- Network adapters with MAC addresses

Serial number and UUID are suppressed for privacy.

{% hint style="info" %}
Log files `Win32_DiskDrive.txt` and `Win32_NetworkAdapter.txt` are written to `$env:TEMP\osdcloud-logs` for later reference.
{% endhint %}

## Syntax

```powershell
Show-OSDCloudDeviceInfo
```

## Parameters

This function has no parameters.

## Examples

```powershell
# Display comprehensive WinPE device and hardware information
Show-OSDCloudDeviceInfo
```

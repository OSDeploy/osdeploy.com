# Update-MyDellBios

Downloads and launches a compatible BIOS update for the current Dell system.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads the latest compatible Dell BIOS update, optionally prepares the
Flash64W utility for WinPE x64 scenarios, suspends BitLocker on the operating
system volume when needed, and launches the BIOS update installer.
The BIOS
installer log is written to $env:TEMP\Update-MyDellBios.log.
Administrative
rights are required.

## Syntax

```powershell
Update-MyDellBios [[-DownloadPath] <String>] [-Force] [-Reboot] [-Silent] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Specifies the directory used to cache the BIOS update and supporting files. The default location is the current user's temporary folder. |
| `-Force` | `SwitchParameter` | False | Forces the update workflow even when the installed BIOS version comparison would not normally trigger an update. |
| `-Reboot` | `SwitchParameter` | False | Adds reboot arguments to the BIOS installer so the system reboots after the silent update completes. |
| `-Silent` | `SwitchParameter` | False | Runs the BIOS installer silently without automatically rebooting the system. |

## Examples

### Example
```powershell
Update-MyDellBios
Downloads and launches the compatible Dell BIOS update with the default
interactive installer behavior.
```

### Example
```powershell
Update-MyDellBios -Silent
Runs the compatible Dell BIOS update silently and does not add a reboot.
```

### Example
```powershell
Update-MyDellBios -Silent -Reboot
Runs the compatible Dell BIOS update silently and requests a reboot when the
installer completes.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

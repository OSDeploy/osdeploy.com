# Add-WindowsPackageSSU

Adds a Servicing Stack Update package to Windows.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Extracts SSU cabinet files from a .cab or .msu package and applies them to an online or offline Windows image using Add-WindowsPackage.

## Syntax

### Offline (Default)
```powershell
Add-WindowsPackageSSU -PackagePath <String> -Path <String> [-LogPath <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Online
```powershell
Add-WindowsPackageSSU -PackagePath <String> [-Online] [-LogPath <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-PackagePath` | `String` | True | Full path to the source .cab or .msu package file. |
| `-Path` | `String` | True | Full path to the root directory of the offline mounted Windows image. |
| `-Online` | `SwitchParameter` | True | Applies the SSU to the currently running operating system. |
| `-LogPath` | `String` | False | Full path to the DISM log file used during package application. |

## Examples

### Example
```powershell
Add-WindowsPackageSSU -PackagePath C:\Updates\windows10.0-kbxxxx.msu -Path C:\Mount
Extracts SSU content from the MSU and applies it to the mounted image at C:\Mount.
```

### Example
```powershell
Add-WindowsPackageSSU -PackagePath C:\Updates\ssu.cab -Online
Applies SSU cab content to the running operating system.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

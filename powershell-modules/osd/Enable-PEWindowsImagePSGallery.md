# Enable-PEWindowsImagePSGallery

Enables PowerShell Gallery in a mounted Windows image

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Configures a mounted Windows image to support PowerShell Gallery by adding necessary registry entries and environment variables to the system profile.

## Syntax

```powershell
Enable-PEWindowsImagePSGallery [[-Path] <String[]>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | False | Path to the mounted Windows image root directory. If not specified, will use the currently mounted image. |

## Examples

### Example
```powershell
Enable-PEWindowsImagePSGallery
Enables PowerShell Gallery in the currently mounted image
```

### Example
```powershell
Enable-PEWindowsImagePSGallery -Path 'C:\Mount'
Enables PowerShell Gallery in the image mounted at C:\Mount
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

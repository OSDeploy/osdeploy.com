# New-OSDCloudOSWimFile

Builds Windows setup media content for an OSDCloud feature update.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves the target operating system image, determines the correct image index for the requested edition, downloads or locates the matching ESD, expands the setup content, and optionally creates an ISO file.

## Syntax

### Default (Default)
```powershell
New-OSDCloudOSWimFile [-OSName <String>] [-OSEdition <String>] [-OSLanguage <String>] [-OSActivation <String>]
 [-CreateISO] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### Legacy
```powershell
New-OSDCloudOSWimFile [-OSEdition <String>] [-OSLanguage <String>] [-OSActivation <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSName` | `String` | False | Specifies the Windows release and architecture to build media for. |
| `-OSEdition` | `String` | False | Specifies the Windows edition to package into the setup media. |
| `-OSLanguage` | `String` | False | Specifies the language and culture of the Windows image. |
| `-OSActivation` | `String` | False | Specifies whether the image should target Retail or Volume activation. |
| `-CreateISO` | `SwitchParameter` | False | Creates an ISO file from the generated setup content after the image is prepared. |

## Examples

### Example
```powershell
New-OSDCloudOSWimFile -OSName 'Windows 11 25H2 x64' -OSEdition Pro -OSLanguage en-us -OSActivation Retail -CreateISO
Prepares the Windows 11 25H2 x64 Pro retail media and builds an ISO file.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://learn.microsoft.com/en-us/windows/deployment/upgrade/log-files](https://learn.microsoft.com/en-us/windows/deployment/upgrade/log-files)
* [https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options?view=windows-11](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options?view=windows-11)

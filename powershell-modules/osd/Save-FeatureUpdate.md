# Save-FeatureUpdate

Downloads the latest matching Windows client feature update package.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Queries OSDCloud operating system metadata using language, activation,
architecture, and either OSName or legacy version/release criteria,
then downloads the newest matching feature update to the specified path.

## Syntax

```powershell
Save-FeatureUpdate [[-DownloadPath] <String>] [[-OSName] <String>] [[-OSActivation] <String>]
 [[-OSArchitecture] <String>] [[-OSLanguage] <String>] [[-OSReleaseID] <String>] [[-OSVersion] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DownloadPath` | `String` | False | Destination directory used to store the downloaded feature update file. Defaults to C:\OSDCloud\OS. |
| `-OSName` | `String` | False | Friendly OS target name used to select a specific version, release, and architecture profile. Defaults to Windows 11 25H2 amd64. |
| `-OSActivation` | `String` | False | Activation channel to filter on. Valid values are Retail and Volume. |
| `-OSArchitecture` | `String` | False | Processor architecture to filter on. Valid values are x64, amd64, and arm64. |
| `-OSLanguage` | `String` | False | Language tag used to filter operating system content. Defaults to en-us. |
| `-OSReleaseID` | `String` | False | Feature update release identifier used with OSVersion for legacy version/release filtering. Examples include 25H2, 24H2, 23H2, and 22H2. |
| `-OSVersion` | `String` | False | Operating system family used with OSReleaseID for legacy version/release filtering. Valid values are Windows 11 and Windows 10. |

## Examples

### Example
```powershell
Save-FeatureUpdate
Downloads the latest matching feature update using default filters.
```

### Example
```powershell
Save-FeatureUpdate -DownloadPath 'D:\OSDCloud\OS' -OSName 'Windows 11 24H2 arm64' -OSLanguage 'en-us' -OSActivation Volume
Downloads the latest matching Windows 11 24H2 arm64 volume feature update to D:\OSDCloud\OS.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-FeatureUpdate

Returns the latest matching Windows client feature update record.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Queries OSDCloud operating system metadata and filters by language, activation,
architecture, and either a named OS target or version/release criteria.
Returns the newest matching feature update object.

## Syntax

```powershell
Get-FeatureUpdate [[-OSName] <String>] [[-OSActivation] <String>] [[-OSArchitecture] <String>]
 [[-OSLanguage] <String>] [[-OSReleaseID] <String>] [[-OSVersion] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSName` | `String` | False | Friendly OS target name used to select a specific version, release, and architecture profile. Defaults to Windows 11 25H2 amd64. |
| `-OSActivation` | `String` | False | Activation channel to filter on. Valid values are Retail and Volume. |
| `-OSArchitecture` | `String` | False | Processor architecture to filter on. Valid values are x64, amd64, and arm64. |
| `-OSLanguage` | `String` | False | Language tag used to filter operating system content. Defaults to en-us. |
| `-OSReleaseID` | `String` | False | Feature update release identifier used with OSVersion for legacy version/release filtering. Examples include 25H2, 24H2, 23H2, and 22H2. |
| `-OSVersion` | `String` | False | Operating system family used with OSReleaseID for legacy version/release filtering. Valid values are Windows 11 and Windows 10. |

## Examples

### Example
```powershell
Get-FeatureUpdate
Returns the latest feature update using default filters.
```

### Example
```powershell
Get-FeatureUpdate -OSName 'Windows 11 24H2 arm64' -OSLanguage 'en-us' -OSActivation Volume
Returns the latest matching arm64 Windows 11 24H2 volume feature update.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

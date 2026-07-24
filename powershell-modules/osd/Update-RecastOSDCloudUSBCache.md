# Update-RecastOSDCloudUSBCache

Starts the Recast OSDCloud command-line deployment workflow.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Initializes device and deployment context, discovers matching operating systems,
resolves driver pack metadata for the current device (or supplied overrides),
validates required dependencies, and prepares global state consumed by
the Recast OSDCloud CLI workflow.

## Syntax

```powershell
Update-RecastOSDCloudUSBCache [[-OSArchitecture] <String>] [[-OSReleaseID] <String>]
 [[-OSLanguageCode] <String>] [[-OSActivation] <String>] [[-OSEdition] <String>] [[-OSDManufacturer] <String>]
 [[-OSDModel] <String>] [[-OSDProduct] <String>] [[-WinPEPostAction] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSArchitecture` | `String` | False | Operating system architecture used when selecting catalog entries. Supported values are amd64 and arm64. |
| `-OSReleaseID` | `String` | False | Operating system release identifier used for catalog selection. |
| `-OSLanguageCode` | `String` | False | Operating system language code used for catalog selection. If not specified, the value is inferred from the current keyboard layout. |
| `-OSActivation` | `String` | False | Operating system activation channel used for catalog selection. |
| `-OSEdition` | `String` | False | Operating system edition used for catalog selection. Valid values depend on OSArchitecture at runtime. |
| `-OSDManufacturer` | `String` | False | Overrides the detected computer manufacturer for driver pack matching. If omitted, the detected device manufacturer is used. |
| `-OSDModel` | `String` | False | Overrides the detected computer model for logging and context alignment. If omitted, the detected device model is used. |
| `-OSDProduct` | `String` | False | Overrides the detected computer product/system ID for driver pack matching. If omitted, the detected device product value is used. |
| `-WinPEPostAction` | `String` | False | Specifies the action to take after the WinPE deployment workflow completes. |

## Examples

### Example
```powershell
Update-RecastOSDCloudUSBCache
Updates the Recast OSDCloud USB cache using detected device values and default deployment selection.
```

### Example
```powershell
Update-RecastOSDCloudUSBCache -OSArchitecture arm64 -OSEdition Pro -OSReleaseID 24H2
Updates the Recast OSDCloud USB cache for an ARM64 Windows 11 Pro 24H2 deployment selection.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Get-OSDCloudDriverPack

Gets the OSDCloud DriverPack for the current or specified computer model

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Gets the OSDCloud DriverPack for the current or specified computer model

## Syntax

```powershell
Get-OSDCloudDriverPack [[-Product] <String>] [[-OSVersion] <String>] [[-OSReleaseID] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Product` | `String` | False | Product is determined automatically by Get-MyComputerProduct |
| `-OSVersion` | `String` | False | No additional description provided. |
| `-OSReleaseID` | `String` | False | No additional description provided. |

## Examples

### Example
```powershell
Get-OSDCloudDriverPack
Returns the most recent matching OSDCloud driver pack for the current device model.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

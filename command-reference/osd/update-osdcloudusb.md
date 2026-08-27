# Update-OSDCloudUSB

Updates resources by using Update-OSDCloudUSB.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for Update-OSDCloudUSB.

## Syntax

```powershell
Update-OSDCloudUSB [[-DriverPack] <String[]>] [-PSUpdate] [-OS] [[-OSLanguage] <String>]
 [[-OSActivation] <String>] [[-OSName] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DriverPack` | `String[]` | False | Specifies the value for DriverPack. |
| `-PSUpdate` | `SwitchParameter` | False | Indicates whether to enable PSUpdate. |
| `-OS` | `SwitchParameter` | False | Indicates whether to enable OS. |
| `-OSLanguage` | `String` | False | Specifies the value for OSLanguage. |
| `-OSActivation` | `String` | False | Specifies the value for OSActivation. |
| `-OSName` | `String` | False | Specifies the value for OSName. |

## Examples

### Example
```powershell
-PSUpdate
Runs Update-OSDCloudUSB with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

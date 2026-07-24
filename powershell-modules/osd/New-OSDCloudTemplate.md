# New-OSDCloudTemplate

Creates resources by using New-OSDCloudTemplate.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides the implementation for New-OSDCloudTemplate.

## Syntax

```powershell
New-OSDCloudTemplate [[-Name] <String>] [[-Language] <String[]>] [[-CumulativeUpdate] <FileInfo>]
 [[-SetAllIntl] <String>] [[-SetInputLocale] <String>] [-WinRE] [-Add7Zip] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Name` | `String` | False | Specifies the value for Name. |
| `-Language` | `String[]` | False | Specifies the value for Language. |
| `-CumulativeUpdate` | `FileInfo` | False | Specifies the value for CumulativeUpdate. |
| `-SetAllIntl` | `String` | False | Specifies the value for SetAllIntl. |
| `-SetInputLocale` | `String` | False | Specifies the value for SetInputLocale. |
| `-WinRE` | `SwitchParameter` | False | Indicates whether to enable WinRE. |
| `-Add7Zip` | `SwitchParameter` | False | Indicates whether to enable Add7Zip. |

## Examples

### Example
```powershell
-Language <Language>
Runs New-OSDCloudTemplate with common parameters.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Save-WebFile

Downloads a file from the internet and returns a Get-Item Object

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Downloads a file from the internet and returns a Get-Item Object

## Syntax

```powershell
Save-WebFile [-SourceUrl] <String> [-DestinationName <String>] [-DestinationDirectory <String>] [-Overwrite]
 [-WebClient] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-SourceUrl` | `String` | True | No additional description provided. |
| `-DestinationName` | `String` | False | No additional description provided. |
| `-DestinationDirectory` | `String` | False | No additional description provided. |
| `-Overwrite` | `SwitchParameter` | False | Overwrite the file if it exists already The default action is to skip the download |
| `-WebClient` | `SwitchParameter` | False | No additional description provided. |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

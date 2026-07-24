# Get-SessionsXml

Returns the Session.xml Updates that have been applied to an Operating System

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns the Session.xml Updates that have been applied to an Operating System

## Syntax

```powershell
Get-SessionsXml [[-Path] <String>] [[-KBNumber] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | False | Specifies the full path to the root directory of the offline Windows image that you will service Or Path of the Sessions.xml file If this value is not set, the running OS Sessions.xml will be processed |
| `-KBNumber` | `String` | False | Returns the KBNumber |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

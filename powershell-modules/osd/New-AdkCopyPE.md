# New-AdkCopyPE

Creates an ADK CopyPE working directory

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Creates a working directory structure for ADK CopyPE media with bootable WinPE environment.

## Syntax

```powershell
New-AdkCopyPE [-Path] <String> [-WinPEArch <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | True | No additional description provided. |
| `-WinPEArch` | `String` | False | No additional description provided. |

## Examples

### Example
```powershell
New-AdkCopyPE -MediaPath 'C:\CopyPEMedia'
Creates a CopyPE working directory at C:\CopyPEMedia
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

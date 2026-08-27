# Get-WindowsAdkPaths

Retrieves the command paths of the Windows Assessment and Deployment Kit (ADK).

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves the command paths of the Windows Assessment and Deployment Kit (ADK).

## Syntax

```powershell
Get-WindowsAdkPaths [[-Architecture] <String>] [[-WindowsAdkRoot] <Object>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Architecture` | `String` | False | Windows ADK architecture to get. Valid values are 'amd64', 'x86', and 'arm64'. |
| `-WindowsAdkRoot` | `Object` | False | Path to the Windows ADK root directory. Typically 'C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit' \[ValidateScript({ if (!($_ \| Test-Path)) { throw 'Path does not exist' } if (!($_ \| Test-Path -PathType Container)) { throw 'Path must be a directory' } if (!(Test-Path "$_\Deployment Tools")) { throw 'Path does not contain a Deployment Tools directory' } if (!(Test-Path "$_\Windows Preinstallation Environment")) { throw 'Path does not contain a Windows Preinstallation Environment directory' } return $true })\] |

## Examples

No examples provided in source documentation.

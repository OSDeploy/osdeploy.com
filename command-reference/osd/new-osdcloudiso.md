# New-OSDCloudISO

Creates an OSDCloud bootable ISO from an OSDCloud workspace.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Validates the local environment and generates an ISO from the workspace
Media directory by calling New-WindowsAdkISO.
If an OSDeploy marker file
exists, the function creates an OSDeploy-labeled ISO for compatibility.

## Syntax

```powershell
New-OSDCloudISO [[-WorkspacePath] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-WorkspacePath` | `String` | False | Path to an OSDCloud workspace that contains Media\sources\boot.wim. If omitted, the current workspace returned by Get-OSDCloudWorkspace is used. |

## Examples

### Example
```powershell
New-OSDCloudISO -WorkspacePath 'C:\OSDCloud'
Creates OSDCloud.iso from C:\OSDCloud\Media.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

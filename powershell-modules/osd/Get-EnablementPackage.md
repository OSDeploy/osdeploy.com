# Get-EnablementPackage

Returns the latest matching Windows enablement package metadata.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Retrieves enablement package metadata from the WSUSXML catalog and filters the result by build and architecture.

## Syntax

```powershell
Get-EnablementPackage [[-OSBuild] <String>] [[-OSArch] <String>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-OSBuild` | `String` | False | Target Windows release build used to filter the enablement package. |
| `-OSArch` | `String` | False | Target operating system architecture used to filter the enablement package. |

## Examples

### Example
```powershell
Get-EnablementPackage -OSBuild 22H2 -OSArch x64
Returns the newest x64 enablement package metadata for 22H2.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

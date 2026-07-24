# Get-WSUSXML

Returns an Array of Microsoft Updates

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Returns an Array of Microsoft Updates contained in the local WSUS Cats

## Syntax

```powershell
Get-WSUSXML [[-Catalog] <String>] [-UpdateArch <String>] [-UpdateBuild <String>] [-UpdateGroup <String>]
 [-UpdateOS <String>] [-GridView] [-Silent] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Catalog` | `String` | False | Filter by Catalog Property |
| `-UpdateArch` | `String` | False | Filter by UpdateArch Property |
| `-UpdateBuild` | `String` | False | Filter by UpdateBuild Property |
| `-UpdateGroup` | `String` | False | Filter by UpdateGroup Property |
| `-UpdateOS` | `String` | False | Filter by UpdateOS Property |
| `-GridView` | `SwitchParameter` | False | Displays the results in GridView with -PassThru |
| `-Silent` | `SwitchParameter` | False | Hide the Current Update Date information |

## Examples

No examples provided in source documentation.

## Related

* [https://osd.osdeploy.com/](https://osd.osdeploy.com/)

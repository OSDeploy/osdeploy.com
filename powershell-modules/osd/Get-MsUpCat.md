# Get-MsUpCat

Retrieves Microsoft updates from the Microsoft Update Catalog

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Searches the Microsoft Update Catalog for updates and returns information about available patches, driver packs, and other updates.

## Syntax

### Search (Default)
```powershell
Get-MsUpCat [-Architecture <String>] [-Descending] [-ExcludeFramework] [-FromDate <DateTime>]
 [-Format <String>] [-GetFramework] [-AllPages] [-IncludeDynamic] [-IncludeFileNames] [-IncludePreview]
 [-LastDays <Int32>] [-MaxSize <Double>] [-MinSize <Double>] [-Properties <String[]>] [-Search] <String>
 [-SizeUnit <String>] [-SortBy <String>] [-Strict] [-ToDate <DateTime>] [-UpdateType <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

### OS
```powershell
Get-MsUpCat [-Architecture <String>] [-Descending] [-ExcludeFramework] [-FromDate <DateTime>]
 [-Format <String>] [-GetFramework] [-AllPages] [-IncludeDynamic] [-IncludeFileNames] [-IncludePreview]
 [-LastDays <Int32>] [-MaxSize <Double>] [-MinSize <Double>] -OperatingSystem <String> [-Properties <String[]>]
 [-SizeUnit <String>] [-SortBy <String>] [-Strict] [-ToDate <DateTime>] [-UpdateType <String[]>]
 [-Version <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Architecture` | `String` | False | Filter results by processor architecture. Valid values are All, x64, x86, or arm64. Default is All. |
| `-Descending` | `SwitchParameter` | False | Sort results in descending order by release date. |
| `-ExcludeFramework` | `SwitchParameter` | False | Exclude .NET Framework updates from results. |
| `-FromDate` | `DateTime` | False | Filter updates from this date |
| `-Format` | `String` | False | Format for the results |
| `-GetFramework` | `SwitchParameter` | False | Only show .NET Framework updates |
| `-AllPages` | `SwitchParameter` | False | Search through all available pages |
| `-IncludeDynamic` | `SwitchParameter` | False | Include dynamic updates |
| `-IncludeFileNames` | `SwitchParameter` | False | Include file names in the results |
| `-IncludePreview` | `SwitchParameter` | False | Include preview updates |
| `-LastDays` | `Int32` | False | Filter updates from the last N days |
| `-MaxSize` | `Double` | False | Filter updates with maximum size |
| `-MinSize` | `Double` | False | Filter updates with minimum size |
| `-OperatingSystem` | `String` | True | Operating System to search updates for |
| `-Properties` | `String[]` | False | Select specific properties to display |
| `-Search` | `String` | True | Search query for Microsoft Update Catalog |
| `-SizeUnit` | `String` | False | Unit for size filtering (MB or GB) |
| `-SortBy` | `String` | False | Sort results by specified field |
| `-Strict` | `SwitchParameter` | False | Use strict search with exact phrase matching |
| `-ToDate` | `DateTime` | False | Filter updates until this date |
| `-UpdateType` | `String[]` | False | Filter by update type |
| `-Version` | `String` | False | endregion Parameters |

## Examples

### Example
```powershell
Get-MsUpCat -Architecture x64
Retrieves x64 updates from Microsoft Update Catalog
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Find-TextInModule

Searches module files for matching text.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves the latest installed version of a module, searches its files for matching text, shows results in Out-GridView, and opens selected files in Visual Studio Code when available.

## Syntax

```powershell
Find-TextInModule [-Text] <String> [[-Module] <String>] [[-Include] <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Text` | `String` | True | Text pattern to search for in module files. |
| `-Module` | `String` | False | Module name to search. The latest installed version is selected. |
| `-Include` | `String[]` | False | File include pattern(s) used by Get-ChildItem during the recursive search. |

## Examples

### Example
```powershell
Find-TextInModule -Text Save-WebFile -Module OSD -Include *.ps1
Searches PowerShell files in the latest installed OSD module for Save-WebFile.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

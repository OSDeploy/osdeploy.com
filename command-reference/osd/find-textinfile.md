# Find-TextInFile

Searches files for matching text and displays selectable results.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Recursively searches files under a path using Select-String, displays matching lines in Out-GridView, and opens selected files in Visual Studio Code when available.

## Syntax

```powershell
Find-TextInFile [-Path] <String> [-Text] <String> [[-Include] <String[]>] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String` | True | Root path to search recursively. |
| `-Text` | `String` | True | Text pattern to search for. |
| `-Include` | `String[]` | False | File include pattern(s) used by Get-ChildItem during the recursive search. |

## Examples

### Example
```powershell
Find-TextInFile -Path C:\Logs -Text Error -Include *.log
Searches all .log files in C:\Logs for Error and shows the matches.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

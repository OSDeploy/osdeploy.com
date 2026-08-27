# Copy-WinREWIM

Copies the Windows Recovery Environment WIM to the specified DestinationDirectory

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Copies the Windows Recovery Environment WIM to the specified DestinationDirectory
This function must be run in Windows

## Syntax

```powershell
Copy-WinREWIM [[-DestinationDirectory] <String>] [[-DestinationFileName] <String>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-DestinationDirectory` | `String` | False | Directory to save the Windows Recovery Environment WIM Default: $env:Temp\sources |
| `-DestinationFileName` | `String` | False | File Name of the Windows Recovery WIM Default: winre.wim |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

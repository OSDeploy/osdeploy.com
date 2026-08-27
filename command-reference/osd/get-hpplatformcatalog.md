# Get-HPPlatformCatalog

Converts the HP Platform list to a PowerShell Object.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Converts the HP Platform list to a PowerShell Object.
Useful to get the computer model name for System Ids
Requires Internet Access to download platformList.cab

## Syntax

```powershell
Get-HPPlatformCatalog [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-HPPlatformCatalog
Don't do this, you will get a big list.
```

### Example
```powershell
$Results = Get-HPPlatformCatalog
Yes do this.  Save it in a Variable
```

### Example
```powershell
Get-HPPlatformCatalog | Out-GridView
Displays all the HP System Ids with the applicable computer model names in GridView
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

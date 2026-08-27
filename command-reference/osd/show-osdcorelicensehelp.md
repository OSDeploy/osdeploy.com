# Show-OSDCoreLicenseHelp

Displays instructions for setting the Recast Core license for OSDCloud.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Provides a concise, step-by-step guide to acquire and place the
Right Click Tools Community Edition license used by OSDCloud.
The function also checks the local license directory and reports
whether any .license2 files are currently present.

## Syntax

```powershell
Show-OSDCoreLicenseHelp [[-LicensePath] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-LicensePath` | `String` | False | The directory path where .license2 files should be stored when not using a full Right Click Tools Community Edition installation. |

## Examples

### Example
```powershell
Show-OSDCoreLicenseHelp
Displays the default setup steps and checks ProgramData\Recast Software\Licenses.
```

### Example
```powershell
Show-OSDCoreLicenseHelp -LicensePath 'D:\Licenses'
Displays setup steps and checks a custom license directory.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://portal.recastsoftware.com/](https://portal.recastsoftware.com/)

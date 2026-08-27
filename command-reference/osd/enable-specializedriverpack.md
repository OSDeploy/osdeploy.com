# Enable-SpecializeDriverPack

Configures driver pack expansion during Windows Specialize phase

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Sets up an unattend XML file to automatically expand driver packs during the Windows Specialize pass.
Requires admin rights and Windows 10 or later.

## Syntax

```powershell
Enable-SpecializeDriverPack [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Enable-SpecializeDriverPack
Configures the system to expand driver packs during Specialize phase
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

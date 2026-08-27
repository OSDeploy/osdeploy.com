# Expand-ZTIDriverPack

Expands driver packs during Lite Touch or Zero Touch deployment

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Processes and extracts driver pack files from C:\Drivers directory during MDT/ConfigMgr task sequence execution.
Supports CAB, EXE, MSI, and ZIP formats from multiple vendors.

## Syntax

```powershell
Expand-ZTIDriverPack [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Expand-ZTIDriverPack
Expands all driver packs in C:\Drivers during task sequence
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

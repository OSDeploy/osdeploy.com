# Expand-StagedDriverPack

Expands staged driver pack archives during Windows Setup

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Extracts and processes staged driver pack files (CAB, EXE, MSI, ZIP) from the C:\Drivers directory.
Supports multiple vendor formats including Dell, HP, Lenovo, and generic packages.

## Syntax

```powershell
Expand-StagedDriverPack [-Apply] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Apply` | `SwitchParameter` | False | Applies drivers during PnP unattend phase |

## Examples

### Example
```powershell
Expand-StagedDriverPack
Expands all driver packs in C:\Drivers
```

### Example
```powershell
Expand-StagedDriverPack -Apply
Expands driver packs and applies them during setup
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

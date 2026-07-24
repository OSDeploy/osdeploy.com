# Backup-DiskToFFU

Captures a physical disk to a Full Flash Update (FFU) image.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Interactively selects a source disk and destination data disk, then uses DISM /Capture-FFU to create an FFU backup from WinPE.

## Syntax

```powershell
Backup-DiskToFFU [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Backup-DiskToFFU
Prompts for source and destination disks, then captures the selected source disk to an FFU file.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)
* [https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/deploy-windows-using-full-flash-update--ffu](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/deploy-windows-using-full-flash-update--ffu)

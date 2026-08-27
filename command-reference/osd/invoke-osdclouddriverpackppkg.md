# Invoke-OSDCloudDriverPackPPKG

Uses DISM in WinPE to expand and apply Driver Packs

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses DISM in WinPE to expand and apply Driver Packs

## Syntax

```powershell
Invoke-OSDCloudDriverPackPPKG [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Invoke-OSDCloudDriverPackPPKG
Applies the packaged OSDCloud driver pack to the Windows image from WinPE.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

# Save-MyBitLockerKeyPackage

Saves BitLocker key packages to destination folders.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Enumerates unlocked BitLocker volumes and exports key package data for each
non-TPM protector to one or more target paths.

## Syntax

```powershell
Save-MyBitLockerKeyPackage [-Path] <String[]> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | True | One or more destination folders used to store exported key packages. |

## Examples

### Example
```powershell
Save-MyBitLockerKeyPackage -Path 'D:\BitLockerBackup'
Exports key package files to D:\BitLockerBackup.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

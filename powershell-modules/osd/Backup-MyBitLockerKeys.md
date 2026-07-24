# Backup-MyBitLockerKeys

Saves available BitLocker key materials to one or more folders.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Calls helper functions to export external keys, key packages, and recovery
passwords for BitLocker-protected volumes.

## Syntax

```powershell
Backup-MyBitLockerKeys [-Path] <String[]> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | True | One or more destination folders used to store exported key materials. |

## Examples

### Example
```powershell
Backup-MyBitLockerKeys -Path 'D:\BitLockerBackup'
Exports BitLocker key materials to D:\BitLockerBackup.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

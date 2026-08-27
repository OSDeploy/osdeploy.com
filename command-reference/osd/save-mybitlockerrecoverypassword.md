# Save-MyBitLockerRecoveryPassword

Saves BitLocker recovery passwords to text files.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Exports recovery password protector values from unlocked volumes and writes
them as recovery key text files in one or more destination folders.

## Syntax

```powershell
Save-MyBitLockerRecoveryPassword [-Path] <String[]> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | True | One or more destination folders used to store recovery password files. |

## Examples

### Example
```powershell
Save-MyBitLockerRecoveryPassword -Path 'D:\BitLockerBackup'
Exports recovery password text files to D:\BitLockerBackup.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

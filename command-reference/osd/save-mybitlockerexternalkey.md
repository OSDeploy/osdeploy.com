# Save-MyBitLockerExternalKey

Saves BitLocker external key protectors (.BEK) to destination folders.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Finds unlocked volumes with external key protectors and exports their
external key files to one or more target paths.

## Syntax

```powershell
Save-MyBitLockerExternalKey [-Path] <String[]> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | True | One or more destination folders used to store exported external keys. |

## Examples

### Example
```powershell
Save-MyBitLockerExternalKey -Path 'D:\BitLockerBackup'
Exports external key files to D:\BitLockerBackup.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

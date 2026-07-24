# Unlock-MyBitLockerExternalKey

Unlocks BitLocker volumes using external key files.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Searches one or more paths for matching .BEK files and unlocks locked
BitLocker volumes that use external key protectors.

## Syntax

```powershell
Unlock-MyBitLockerExternalKey [[-Path] <String[]>] [-Recurse] [-ProgressAction <ActionPreference>]
 [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Path` | `String[]` | False | One or more folders to search for matching .BEK external key files. |
| `-Recurse` | `SwitchParameter` | False | Searches subdirectories under each path for matching key files. |

## Examples

### Example
```powershell
Unlock-MyBitLockerExternalKey -Path 'D:\BitLockerBackup'
Unlocks volumes using matching .BEK files in the specified folder.
```

### Example
```powershell
Unlock-MyBitLockerExternalKey -Path 'D:\BitLockerBackup' -Recurse
Unlocks volumes using matching .BEK files found recursively.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

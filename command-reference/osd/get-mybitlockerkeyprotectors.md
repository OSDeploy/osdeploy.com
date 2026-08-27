# Get-MyBitLockerKeyProtectors

Returns BitLocker key protector details for encrypted volumes.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Enumerates BitLocker volumes and returns protector metadata, with optional
inclusion of recovery password values.

## Syntax

```powershell
Get-MyBitLockerKeyProtectors [-ShowRecoveryPassword] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ShowRecoveryPassword` | `SwitchParameter` | False | Includes recovery password values in the output when specified. |

## Examples

### Example
```powershell
Get-MyBitLockerKeyProtectors
Lists key protector details without recovery password values.
```

### Example
```powershell
Get-MyBitLockerKeyProtectors -ShowRecoveryPassword
Lists key protector details including recovery password values.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

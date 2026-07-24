# Set-BootmgrTimeout

Sets the Windows Boot Manager timeout value in BCD.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Updates the '{bootmgr}' timeout entry in BCD using bcdedit.
This controls
how many seconds the boot menu waits before selecting the default entry.

## Syntax

```powershell
Set-BootmgrTimeout [-Timeout] <UInt32> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Timeout` | `UInt32` | True | Timeout value in seconds to set on the Boot Manager entry. |

## Examples

### Example
```powershell
Set-BootmgrTimeout -Timeout 10
Sets the Boot Manager timeout to 10 seconds.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

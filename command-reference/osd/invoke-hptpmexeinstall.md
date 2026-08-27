# Invoke-HPTPMEXEInstall

Extracts and installs the HP TPM firmware update from C:\OSDCloud\HP\TPM.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Locates the firmware EXE in C:\OSDCloud\HP\TPM, silently extracts it, then runs
TPMConfig64.exe with the specified arguments to apply the TPM firmware update.
Logs activity to C:\Windows\TEMP\osdcloud-logs\TPMConfig.log.
Outputs the exit code from
TPMConfig64 along with a human-readable description for all documented exit codes.

## Syntax

```powershell
Invoke-HPTPMEXEInstall [[-path] <Object>] [[-filename] <Object>] [[-spec] <Object>] [[-logsuffix] <Object>]
 [[-WorkingFolder] <Object>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-path` | `Object` | False | Reserved parameter. Not currently used. |
| `-filename` | `Object` | False | Optional firmware binary filename passed to TPMConfig64 via the -f argument. |
| `-spec` | `Object` | False | Optional TPM specification version to target (e.g., '1.2' or '2.0'). Passed to TPMConfig64 via the -a argument. |
| `-logsuffix` | `Object` | False | Reserved parameter. Not currently used. |
| `-WorkingFolder` | `Object` | False | Reserved parameter. Not currently used. |

## Examples

### Example
```powershell
Invoke-HPTPMEXEInstall
Installs the TPM firmware using default TPMConfig64 arguments.
```

### Example
```powershell
Invoke-HPTPMEXEInstall -spec '1.2'
Installs the TPM firmware targeting the 1.2 specification.
```

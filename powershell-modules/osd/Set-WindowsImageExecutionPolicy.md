# Set-WindowsImageExecutionPolicy

Sets the PowerShell Execution Policy of a mounted Windows Image

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Sets the PowerShell Execution Policy of a mounted Windows Image

## Syntax

```powershell
Set-WindowsImageExecutionPolicy [-ExecutionPolicy] <String> [-Path <String[]>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ExecutionPolicy` | `String` | True | Specifies the new execution policy. The acceptable values for this parameter are: - Restricted. Does not load configuration files or run scripts. Restricted is the default execution policy. - AllSigned. Requires that all scripts and configuration files be signed by a trusted publisher, including scripts that you write on the local computer. - RemoteSigned. Requires that all scripts and configuration files downloaded from the Internet be signed by a trusted publisher. - Unrestricted. Loads all configuration files and runs all scripts. If you run an unsigned script that was downloaded from the Internet, you are prompted for permission before it runs. - Bypass. Nothing is blocked and there are no warnings or prompts. - Undefined. Removes the currently assigned execution policy from the current scope. This parameter will not remove an execution policy that is set in a Group Policy scope. |
| `-Path` | `String[]` | False | Specifies the full path to the root directory of the offline Windows image that you will service If a Path is not specified, all mounted Windows Images will be modified |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

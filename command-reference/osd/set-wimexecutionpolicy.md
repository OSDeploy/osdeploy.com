# Set-WimExecutionPolicy

Sets the PowerShell Execution Policy of a Windows Image .wim file (Mount | Set | Dismount -Save)

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Sets the PowerShell Execution Policy of a Windows Image .wim file (Mount | Set | Dismount -Save)

## Syntax

```powershell
Set-WimExecutionPolicy [-ExecutionPolicy] <String> -ImagePath <String[]> [-Index <UInt32>]
 [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-ExecutionPolicy` | `String` | True | Specifies the new execution policy. The acceptable values for this parameter are: - Restricted. Does not load configuration files or run scripts. Restricted is the default execution policy. - AllSigned. Requires that all scripts and configuration files be signed by a trusted publisher, including scripts that you write on the local computer. - RemoteSigned. Requires that all scripts and configuration files downloaded from the Internet be signed by a trusted publisher. - Unrestricted. Loads all configuration files and runs all scripts. If you run an unsigned script that was downloaded from the Internet, you are prompted for permission before it runs. - Bypass. Nothing is blocked and there are no warnings or prompts. - Undefined. Removes the currently assigned execution policy from the current scope. This parameter will not remove an execution policy that is set in a Group Policy scope. |
| `-ImagePath` | `String[]` | True | Specifies the location of the WIM or VHD file containing the Windows image you want to mount. |
| `-Index` | `UInt32` | False | Index of the WIM to Mount Default is 1 |

## Examples

No examples provided in source documentation.

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

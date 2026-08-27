# Export-OSDCertificatesAsReg

Exports selected LocalMachine certificates as .reg files.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Prompts for installed certificates and exports matching certificate registry keys from system certificate hives into .reg files under the temporary Certs folder.

## Syntax

```powershell
Export-OSDCertificatesAsReg [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Export-OSDCertificatesAsReg
Opens a selection grid and exports registry-backed certificate entries for selected certificates to $env:Temp\Certs.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

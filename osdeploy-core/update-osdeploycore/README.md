---
description: Update all Windows, recovery, and driver content used by OSDeploy Core.
---

# Update OSDeployCore

`Update-OSDeployCore` is the complete OSDeploy Core update function in the OSDeploy PowerShell module. Run it to download current Windows source files, prepare Windows and Windows Recovery Environment (WinRE) content, and update Windows Preinstallation Environment (WinPE) drivers.

{% hint style="info" %}
**TLDR:** Open PowerShell 7 as an administrator, then run:

```powershell
Update-OSDeployCore
```

The function runs all three OSDeploy Core update stages in the correct order.
{% endhint %}

## Run the Complete Update

Run the following command:

```powershell
Update-OSDeployCore
```

The complete update runs these public sub-functions in sequence:

| Stage | Public sub-function | Purpose |
| ----- | ------------------- | ------- |
| 1 | `Update-OSDeployCoreESD` | Downloads and verifies the Windows 11 Enterprise Electronic Software Download (ESD) source files. |
| 2 | `Update-OSDeployCoreOS` | Uses the ESD files to prepare Windows setup media and WinRE content. |
| 3 | `Update-OSDeployCoreDrivers` | Downloads and prepares current WinPE driver packages. |

Windows ESD files are large, and preparing the Windows images can take time. Follow any download prompts and leave the PowerShell window open until all three stages finish.

Preview the complete update without downloading or importing content:

```powershell
Update-OSDeployCore -WhatIf
```

## Update One Stage

Each stage is also a public function in the OSDeploy PowerShell module. Run a sub-function directly when only that part of OSDeploy Core needs updating.

Update only the Windows ESD source files:

```powershell
Update-OSDeployCoreESD
```

Update only the prepared Windows and WinRE content:

```powershell
Update-OSDeployCoreOS
```

Update only the WinPE drivers:

```powershell
Update-OSDeployCoreDrivers
```

The sub-functions provide additional options for selecting an architecture, driver source, or download behavior. Use the stage guides for those options.

## Next Step

After OSDeploy Core is updated, build the boot image:

```powershell
Build-OSDeployBoot
```

## In This Section

* [Update Windows 11 ESD](update-windows-11-esd/) - Run stage one independently and manage the Windows ESD downloads.
* [Update Windows 11 OS](update-windows-11-os/) - Run stage two independently and prepare Windows and WinRE content.
* [Update WinPE Drivers](update-winpe-drivers/) - Run stage three independently and manage WinPE drivers.

## Related

* [OSDeploy PowerShell Module](../../powershell-modules/osdeploy/)
* [Update-OSDeployCore command reference](../../powershell-modules/osdeploy/Update-OSDeployCore.md)
* [Build-OSDeployBoot command reference](../../powershell-modules/osdeploy/Build-OSDeployBoot.md)
* [System Requirements](../../osdeploy-pc/system-requirements.md)

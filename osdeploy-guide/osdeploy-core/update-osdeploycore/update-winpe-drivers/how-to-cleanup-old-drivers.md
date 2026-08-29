---
description: Remove obsolete WinPE driver versions from the OSDeploy Core library.
---

# How To: Driver Cleanup

`Update-OSDeployCoreDrivers` stores each driver release in a separate versioned folder. New releases do not replace older folders. Over time, the library can contain multiple Dell, HP, Intel, and Microsoft driver versions.

Remove obsolete versions to reduce disk usage and keep the driver selection list in `Build-OSDeployBoot` easy to identify.

{% hint style="warning" %}
Keep the newest known-working version of each driver family and architecture. Do not delete the `amd64` or `arm64` architecture folders. A newer version number is usually the current release, but verify the version you use before deleting an older folder.
{% endhint %}

## Driver Folder Location

Expanded driver folders are stored by architecture:

```
C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\amd64\
C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\arm64\
```

A folder name contains the driver family and version. For example:

```
dell-A09
dell-A10
hp-3.30
hp-3.40
intel-ethernet-31.1
intel-ethernet-31.2.2
```

In these examples, `dell-A09`, `hp-3.30`, and `intel-ethernet-31.1` are older candidates. Keep a previous release when it is still required for tested hardware or rollback.

## Review Installed Drivers

Close any active OSDeploy build, then open an elevated PowerShell 7 session.

List the expanded folders for both architectures:

```powershell
$DriverRoot = "$env:ProgramData\OSDeployCore\OSDRepo\winpe-drivers"

Get-ChildItem -Path "$DriverRoot\*\*" -Directory -ErrorAction SilentlyContinue |
	Select-Object Name, Parent, LastWriteTime, FullName |
	Format-Table -AutoSize
```

Compare folders belonging to the same driver family. Use the version in the folder name to identify the release. Treat `LastWriteTime` only as supporting information because copying or re-expanding a folder can change that date.

## Select Old Driver Folders

Use `Out-GridView` to select only the obsolete versioned folders. Hold **Ctrl** to select multiple rows, then select **OK**.

```powershell
$OldDrivers = Get-ChildItem -Path "$DriverRoot\*\*" -Directory -ErrorAction SilentlyContinue |
	Select-Object Name,
		@{Name = 'Architecture'; Expression = { $_.Parent.Name } },
		LastWriteTime,
		FullName |
	Out-GridView -Title 'Select old WinPE driver folders to remove' -PassThru
```

Review the selection before deleting anything:

```powershell
$OldDrivers | Format-Table Name, Architecture, FullName -AutoSize
```

If the selection is incorrect, run the selection command again.

## Preview the Removal

Run `Remove-Item` with `-WhatIf` to preview every folder that would be deleted:

```powershell
$OldDrivers.FullName | Remove-Item -Recurse -Force -WhatIf
```

Confirm that every displayed path is below the correct architecture folder and represents an obsolete version.

## Remove the Old Drivers

Remove the selected folders and confirm each deletion:

```powershell
$OldDrivers.FullName | Remove-Item -Recurse -Force -Confirm
```

The command deletes only the selected expanded folders. It does not remove the `amd64` or `arm64` parent folders.

{% hint style="info" %}
Downloaded package archives are stored separately below `C:\ProgramData\OSDeployCore\cache\downloads`. Removing an expanded driver folder does not remove its downloaded archive. When the current catalog still references that package, rerunning `Update-OSDeployCoreDrivers` can restore the expanded folder from the cached archive.
{% endhint %}

## Verify the Cleanup

List the remaining folders:

```powershell
Get-ChildItem -Path "$DriverRoot\*\*" -Directory -ErrorAction SilentlyContinue |
	Select-Object Name, Parent, FullName |
	Format-Table -AutoSize
```

Confirm that one current or known-working version remains for each required driver family. Run `Build-OSDeployBoot` and select only the required driver folders when creating the next boot image.

## Restore a Deleted Driver

Refresh a current driver source when a required folder was removed accidentally:

```powershell
Update-OSDeployCoreDrivers -Name 'dell'
```

Replace `dell` with `hp`, `intel-ethernet`, or `intel-wifi` as required. The command restores the version currently published in the OSDeploy driver catalog; it might not restore an older vendor release that is no longer published.

## Related

* [Update WinPE Drivers](./)
* [Update-OSDeployCoreDrivers command reference](../../../../command-reference/osdeploy/update-osdeploycoredrivers.md)

---
description: Refresh Windows ESD, Windows OS and recovery, and WinPE driver content in the required order.
---

# Update-OSDeployCore

`Update-OSDeployCore` runs the complete OSDeploy Core content refresh. It downloads and verifies catalog-selected Windows Enterprise ESD files, imports those files as Windows OS and Windows Recovery Environment (WinRE) sources, and then refreshes the WinPE driver catalog and packages.

Use the individual stage commands when you need architecture, source, force, or download-only controls. The orchestrator has no stage-specific parameters.

## Requirements

Run the command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

Install or update the [OSDeploy module](../requirements/powershell-modules.md) before refreshing Core content. Internet access is required for uncached ESD files, driver catalog discovery, and uncached driver packages. Allow substantial free space under `%ProgramData%\OSDeployCore` for source ESDs, expanded setup media, recovery images, and drivers.

{% hint style="warning" %}
The command stops before the first update stage when a Windows version, PowerShell, MSI installation, `curl.exe`, or administrator check fails.
{% endhint %}

## Parameters

`Update-OSDeployCore` has no command-specific parameters. It supports common parameters, including `-Verbose`, and the following `SupportsShouldProcess` parameters:

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-WhatIf` | Common parameter | Not enabled | Passes preview behavior to all three stage commands. Requirement checks, Core path initialization, cache inspection, prompts, and network discovery can still occur. |
| `-Confirm` | Common parameter | Not enabled | Flows the confirmation preference to the stage commands. The orchestrator does not present one confirmation for the complete workflow. |

## Examples

### Refresh all Core content

Run all three update stages in dependency order:

```powershell
Update-OSDeployCore
```

Follow the per-file ESD prompts. Driver packages are processed automatically unless confirmation is requested through a common preference.

### Preview the complete workflow

Run requirement checks, selection, cache inspection, and discovery while suppressing operations protected by `ShouldProcess`:

```powershell
Update-OSDeployCore -WhatIf
```

This is not a read-only preview. See [WhatIf and Confirmation](#whatif-and-confirmation) for the operations that occur before a `ShouldProcess` boundary.

### Capture all returned objects

Capture verified ESD files, newly imported OS directories, and processed driver package files in invocation order:

```powershell
$CoreResults = Update-OSDeployCore
$CoreResults | Select-Object FullName, PSIsContainer
```

## Orchestration Order

The command initializes the OSDeploy Core paths, then invokes these public commands synchronously:

| Stage | Command | Behavior |
| --- | --- | --- |
| 1 | `Update-OSDeployCoreESD` | Selects the newest bundled OS catalog, resolves host-supported en-US Enterprise ESD entries, reuses verified cache files, and prompts before each required download. |
| 2 | `Update-OSDeployCoreOS` | Reads only ESDs that match the newest catalog checksum, imports Windows setup and WinRE content, and stages Microsoft inbox network drivers. |
| 3 | `Update-OSDeployCoreDrivers` | Refreshes all active WinPE driver sources, then downloads and expands all matching packages. Wi-Fi packages are excluded automatically when no valid imported WinRE source exists. |

The order is fixed. Stage two does not download a missing ESD itself, so stage one must complete first. Stage three runs after OS import so a newly created WinRE source can make Wi-Fi packages eligible.

The orchestrator does not catch stage errors. A terminating error stops the workflow and later stages do not run. Declined, unavailable, or unsuccessful individual ESD downloads are normally omitted rather than treated as terminating errors; stage two then imports whichever current-catalog ESDs are verified in the cache.

## File Effects

Before the stages run, Core path initialization can create the standard cache and repository directory tree under `%ProgramData%\OSDeployCore`. It can also migrate legacy repository folders and build profiles, normalize profile names and properties, and rewrite persisted paths that refer to legacy locations.

The update stages can then change these primary locations:

| Location | Content |
| --- | --- |
| `%ProgramData%\OSDeployCore\OSDCloud\OS\<Windows release>` | Downloaded Enterprise ESD files selected from the bundled catalog. |
| `%ProgramData%\OSDeployCore\cache\windows-os` | Imported Windows setup media, WIM files, metadata, supporting files, and logs. |
| `%ProgramData%\OSDeployCore\cache\windows-re` | Recovery images and metadata derived from the matching Windows OS imports. |
| `%ProgramData%\OSDeployCore\cache\config\winpedrivers.json` | Merged local WinPE driver catalog. |
| `%ProgramData%\OSDeployCore\cache\downloads` | Cached vendor driver package archives. |
| `%ProgramData%\OSDeployCore\repository\build-winpedrivers` | Expanded vendor packages and Microsoft inbox network drivers used by boot builds. |

Existing valid content is reused by default. See the stage guides for checksum fallback, duplicate-import, catalog-merge, force, and expansion behavior.

## WhatIf and Confirmation

`Update-OSDeployCore` declares `SupportsShouldProcess`, but it does not call `ShouldProcess` itself. PowerShell preference variables carry `-WhatIf` and `-Confirm` into the three stage commands, where each operation defines its own boundary.

With `-WhatIf`:

* Core path initialization still runs and is not protected by `ShouldProcess`.
* ESD catalog parsing, cache hashing, older-cache lookup, URL reachability tests, and `ShouldContinue` prompts can still occur. Verified cached ESD files can still be returned.
* OS source discovery and checksum verification still occur, but each import is skipped at its destination-level `ShouldProcess` call.
* Driver source discovery can contact vendor endpoints. The driver catalog directory can be created, but the catalog write, package download, and expansion are skipped.

With `-Confirm`, there is no single all-stages approval. ESD uses its own Yes/No prompts and `ShouldProcess` calls, OS import confirms per destination, and the driver stage confirms its combined catalog-and-package operation. The driver stage suppresses additional confirmation prompts in its child commands.

{% hint style="warning" %}
Do not use `-WhatIf` as a guarantee that the local Core tree and saved build profiles remain unchanged. Run it only after reviewing the initialization effects above.
{% endhint %}

## Output

The function passes pipeline output from each child command through in stage order. A run can return a mixed collection:

| Type | Source and meaning |
| --- | --- |
| `System.IO.FileInfo` | Current-catalog ESD files that were downloaded or verified, an older verified ESD retained after declining an upgrade, and driver package archives processed successfully. |
| `System.IO.DirectoryInfo` | Windows OS destination directories created by completed imports. |

Existing OS imports, declined or failed downloads, unmatched driver sources, and skipped packages produce no result object. Host, warning, verbose, and `WhatIf` messages are not pipeline output.

See [Update Core Content](../basic/update-osdeploycore.md) for the shortest workflow, or continue with the stage guides for [ESD downloads](update-osdeploycoreesd.md), [OS import](update-osdeploycoreos.md), and [WinPE drivers](update-osdeploycoredrivers.md). For compact syntax, see the [Update-OSDeployCore command reference](../../command-reference/osdeploy/update-osdeploycore.md).

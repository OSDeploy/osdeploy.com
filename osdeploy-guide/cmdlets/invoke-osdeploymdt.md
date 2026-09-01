---
description: Apply OSDeploy customizations at MDT LiteTouchPE exit stages.
---

# Invoke-OSDeployMDT

`Invoke-OSDeployMDT` reads the MDT LiteTouchPE exit environment and applies OSDeploy customizations during **Update Deployment Share**. `Install-OSDeployMDT` registers the command in the deployment share template so MDT calls it at the `WIM`, `POSTWIM`, `ISO`, and `POSTISO` stages.

## Requirements

Run this command through the MDT **Update Deployment Share** process on a host with MDT, the Windows ADK, the WinPE add-on, DISM, Windows PowerShell, PowerShell 7, and internet access for uncached tools and modules. The WIM stage also uses `curl.exe`, `robocopy.exe`, `reg.exe`, PowerShellGet, and Windows image servicing commands.

Initialize the deployment share first with [Install-OSDeployMDT](install-osdeploymdt.md). See [Microsoft Deployment Toolkit](../../core-components/microsoft-deployment-toolkit/README.md) for the related setup.

{% hint style="warning" %}
Do not simulate an MDT stage against an arbitrary path. The function assumes that MDT supplied a valid mounted image or ISO build tree and does not validate every environment value before changing content.
{% endhint %}

The implemented WIM app and driver paths are `amd64` oriented. AzCopy and curl download only amd64 payloads, 7-Zip copies only its x64 payload, repository driver selection reads `build-winpedrivers\amd64`, and driver log entries record `amd64`.

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-SetInputLocale` | `String` | `en-us` | Set the WinPE input locale passed to DISM. The function always sets all other international settings to `en-us`. |
| `-SetTimeZone` | `String` | Current timezone from `tzutil /g` | Use a timezone name present in `tzutil /l`. Validation occurs before stage handling. |
| `-WhatIf` | Common parameter | Not enabled | At `WIM` or `POSTISO`, skip that stage's customization body. Environment display, global build-context creation, the final delay, and an enabled exit prompt still occur. |
| `-Confirm` | Common parameter | Not enabled | At `WIM` or `POSTISO`, prompt before that stage's customization body. Other recognized or unrecognized nonempty stages have no confirmation boundary. |

## Examples

### Display help outside MDT

Run the command without an MDT `STAGE` environment variable to return its help object without creating a build context or changing content:

```powershell
Remove-Item Env:\STAGE -ErrorAction SilentlyContinue
Invoke-OSDeployMDT
```

### Use the MDT-provided defaults

The exit command registered by `Install-OSDeployMDT` runs this invocation at each MDT stage. During `WIM`, it applies `en-us` international and input settings and uses the host's current timezone:

```powershell
Invoke-OSDeployMDT
```

### Set the WinPE keyboard and timezone

Use valid DISM locale and Windows timezone values when invoking the command from the MDT exit environment:

```powershell
Invoke-OSDeployMDT `
	-SetInputLocale 'en-GB' `
	-SetTimeZone 'GMT Standard Time'
```

To make these values persistent, include the parameters in the `Invoke-OSDeployMDT` exit entry in `DEPLOYROOT\Templates\LiteTouchPE.xml`. Running `Install-OSDeployMDT -Force` later overwrites that template from the MDT installation before restoring only the default exit command.

### Preview a WIM stage

Use `-WhatIf` when MDT has supplied the current WIM-stage environment. The function creates and leaves the global build context but skips the WIM customization body:

```powershell
Invoke-OSDeployMDT -WhatIf
```

## MDT Environment

When `STAGE` is absent or empty, the function runs `Get-Help -Name Invoke-OSDeployMDT` and returns immediately. With any nonempty value, it displays these MDT variables and replaces `$global:BuildMedia` with the current build context:

| Variable | Use |
| --- | --- |
| `STAGE` | Selects stage behavior. Recognized values are `WIM`, `POSTWIM`, `ISO`, and `POSTISO`. |
| `CONTENT` | Becomes the mounted image path at `WIM`; at `POSTISO`, identifies the captured ISO used to derive the temporary ISO tree and output name. |
| `ARCHITECTURE` | Records the build architecture and selects cached app content. Current app and driver implementation is amd64 oriented. |
| `DEPLOYROOT` | Locates `Boot\bootbins`, template drivers, driver logs, and the patched ISO output. |
| `INSTALLDIR` | Locates MDT's `Templates\BootOrder.txt` for ISO creation. |
| `ADKPath` | Displayed for diagnostics. ADK tool paths are independently resolved from the Windows Kits registry or default installation path. |
| `TEMPLATE` | Displayed for diagnostics; it does not select a code branch. |
| `TEMP` | Stores DISM logs and temporary INF files. |
| `ProgramData` | Locates OSDeploy caches for PowerShell packages, WinPE apps, and repository drivers. |

For the normal MDT exit flow, `CONTENT` has these meanings:

| Stage | `CONTENT` value | Implemented action |
| --- | --- | --- |
| `WIM` | Mounted WinPE image directory | Apply WIM customizations. |
| `POSTWIM` | Captured WIM file | None. |
| `ISO` | ISO staging directory | None. |
| `POSTISO` | Captured ISO file | Patch the sibling temporary `ISO` tree and build a CA 2023 ISO. |

An unrecognized nonempty `STAGE` also performs no stage-specific action. It still replaces `$global:BuildMedia`, waits 10 seconds, and can prompt on exit.

## WIM Stage

At `STAGE=WIM`, `CONTENT` is the mounted WinPE root. One `ShouldProcess` call gates the complete WIM body.

### Boot and ADK files

The function creates `DEPLOYROOT\Boot\bootbins` and copies available files into it:

* `bootmgfw.efi` and `bootmgfw_EX.efi` from the mounted image.
* `efisys.bin`, `efisys_noprompt.bin`, `efisys_EX.bin`, `efisys_noprompt_EX.bin`, and `etfsboot.com` from the ADK Oscdimg directory.

Missing boot files are skipped. It also copies `oa3tool.exe` from the ADK into `Windows\System32` when that source exists.

### WinPE settings and PowerShell support

DISM sets the timezone, sets all international settings to `en-us`, sets the requested input locale, and writes timestamped logs under the host's `TEMP` directory.

The PowerShell preparation step:

* Creates default-user, system-profile, module, script, and repository folders in the mounted image.
* Uses `C:\ProgramData\OSDeployCore\cache\psrepository` as a host cache for PackageManagement 1.4.8.1, PowerShellGet 2.2.5, and `nuget.exe`.
* Unregisters any existing `OSDeployCore` PowerShell repository, registers the cache under that trusted name, saves PackageManagement and PowerShellGet into WinPE, then unregisters it. The command therefore leaves no `OSDeployCore` repository registered in the host session.
* Creates WinPE PowerShell repository, profile, environment, and execution-policy content where needed.
* Loads and changes the mounted default-user registry hive and injects two temporary INF drivers for environment and execution-policy values.
* Saves the current `OSDCloud` module from the PowerShell Gallery into `Program Files\WindowsPowerShell\Modules` in the image.

These operations can download content and change host-side caches as well as the mounted image.

### WinPE tools

When the corresponding executable is absent from mounted `Windows\System32`, the function uses `C:\ProgramData\OSDeployCore\cache\winpe-apps` to cache and copy:

* AzCopy v10 for Windows.
* The latest curl for Windows amd64 package.
* 7-Zip 26.00 x64 extra tools.

The 7-Zip step removes older cached 7-Zip versions. Existing executables in the mounted image cause their installer step to return without refreshing the cache.

### WinPE drivers and logs

The function reads `CONTENT\winpe-drivers.json` when present. Invalid JSON warns and starts with an empty applied-driver list. When the mounted log is absent, the function resets the list and removes an existing `DEPLOYROOT\Boot\winpe-drivers.json`.

Driver selection then follows this order:

1. Enumerate each immediate subdirectory under `DEPLOYROOT\Templates\winpe-drivers` and apply it automatically with recursive, unsigned-driver servicing unless its folder name is already logged.
2. Enumerate unapplied immediate subdirectories under the OSDeploy repository's `build-winpedrivers\amd64` path.
3. When repository drivers are available, open `Out-GridView` with multi-selection. Cancel the picker to skip these optional drivers.

Deduplication uses the driver folder `Name`, not the full path or file contents. Applied entries are written to `CONTENT\winpe-drivers.json` and copied to `DEPLOYROOT\Boot\winpe-drivers.json`. `Add-WindowsDriver` servicing objects can reach the success pipeline.

## POSTISO Stage

At `STAGE=POSTISO`, one `ShouldProcess` call gates the complete patch-and-build body. The function uses the parent directory of `CONTENT` and its sibling `ISO` folder as the source tree. It derives the output base name from the captured ISO and writes:

```text
DEPLOYROOT\Boot\<IsoBaseName>_uefi2023ca.iso
```

If `DEPLOYROOT\Boot\bootbins\bootmgfw_EX.efi` exists, it overwrites `ISO\EFI\MICROSOFT\BOOT\bootmgfw.efi`. If that source is missing, the function warns but continues to ISO creation.

When ADK `oscdimg.exe` exists, the function starts it and waits for it to finish. The boot definition uses MDT's `Templates\BootOrder.txt`, ADK `etfsboot.com`, and `DEPLOYROOT\Boot\bootbins\efisys_EX.bin`. The function does not inspect or return the `oscdimg` process exit code. A missing `oscdimg.exe` produces a warning and no patched ISO.

## Prompts and Exit Behavior

The WIM repository-driver picker appears only when unapplied repository drivers exist. Canceling it skips those drivers and continues.

After every invocation with a nonempty `STAGE`, the function waits 10 seconds. If a truthy `PauseOnExit` variable is visible in caller scope, it then prompts `Press Enter to continue`; `PauseOnExit` is not a command parameter, and its displayed value does not control the fixed 10-second wait. Invocations without `STAGE` return before this delay and prompt.

## WhatIf and Confirmation

`ShouldProcess` is implemented only for `WIM` and `POSTISO`:

* `-WhatIf` skips all work inside the matching stage body, including downloads, mounted-image changes, driver prompts, and ISO creation.
* `-Confirm` prompts once for the matching stage body.
* Both parameters act after environment display and `$global:BuildMedia` replacement and before the final delay or optional exit prompt.
* `POSTWIM`, `ISO`, and unrecognized nonempty stages do not call `ShouldProcess` because they currently have no stage-specific body.

## Output

When `STAGE` is absent, the function returns the `System.Management.Automation.HelpInfo` object emitted by `Get-Help`.

The function has no single structured stage result. During `WIM`, `Add-WindowsDriver` can emit `Microsoft.Dism.Commands.BasicDriverObject` values, and delegated or native commands can emit incidental success-stream output. Host messages and `$global:BuildMedia` are not intentional pipeline results. `POSTISO` does not return the created ISO or its process exit code.

See the [MDT invocation workflow](../../osdeploy-mdt-integration/invoke-osdeploymdt.md), the [Install-OSDeployMDT guide](install-osdeploymdt.md), or the [Invoke-OSDeployMDT command reference](../../command-reference/osdeploy/invoke-osdeploymdt.md).

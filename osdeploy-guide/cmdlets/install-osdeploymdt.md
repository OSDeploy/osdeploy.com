---
description: Audit or initialize an MDT deployment share for OSDeploy customization.
---

# Install-OSDeployMDT

`Install-OSDeployMDT` selects a registered MDT deployment share, audits its OSDeploy configuration, and optionally writes the templates and settings that run `Invoke-OSDeployMDT` during **Update Deployment Share**.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

Install MDT on the same computer and register at least one persistent MDT deployment share. See [Microsoft Deployment Toolkit](/broken/pages/waD3ryEEjkQj6wqEhrGA) and [Module Setup](../requirements/powershell-modules.md).

{% hint style="warning" %}
`-Force` overwrites deployment-share template files. Review audit output before applying changes, and preserve any custom template content that must be restored afterward.
{% endhint %}

## Parameters

| Parameter  | Type              | Default     | Accepted values and behavior                                                                                                                                                            |
| ---------- | ----------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-Force`   | `SwitchParameter` | Not enabled | Create missing folders, copy or overwrite templates, and apply supported XML changes. When omitted, report applicable work without changing the selected share's templates or settings. |
| `-WhatIf`  | Common parameter  | Not enabled | Perform prerequisite and deployment-share discovery, then display the top-level initialization operation without running the share audit or initialization body.                        |
| `-Confirm` | Common parameter  | Not enabled | Prompt before running the audit or initialization body for the selected deployment share.                                                                                               |

The function does not accept a deployment-share path. It always selects from persistent MDT drives registered on the local computer.

## Examples

### Audit a deployment share

Select a deployment share and report existing, missing, or differing OSDeploy content without changing its templates or `Settings.xml`:

```powershell
Install-OSDeployMDT
```

### Initialize a deployment share

Apply the required folder, template, exit-command, component, and settings changes:

```powershell
Install-OSDeployMDT -Force
```

### Confirm before initialization

Select the share, then require confirmation before writing its content:

```powershell
Install-OSDeployMDT -Force -Confirm
```

### Preview the top-level operation

Resolve and select the deployment share, then stop before auditing or changing it:

```powershell
Install-OSDeployMDT -Force -WhatIf
```

`-WhatIf` does not suppress deployment-share discovery or selection side effects described below.

## Deployment Share Selection

The function resolves the MDT installation directory from `HKLM:\SOFTWARE\Microsoft\Deployment 4`, value `Install Dir`. If that value is unavailable or its path does not exist, it tries `C:\Program Files\Microsoft Deployment Toolkit`. The function warns and returns when neither path exists.

In PowerShell 7, OSDeploy starts `powershell.exe` noninteractively to load the MDT module and enumerate persistent MDT drives. Selection then follows these rules:

1. Return without changing a share when no persistent drives are found.
2. Remove persistent-drive registrations whose local paths no longer exist.
3. Select the only reachable share automatically.
4. When multiple shares remain, display a numbered console list and prompt with `Enter number`.
5. Return when the response is invalid, outside the displayed range, or no share is selected.

Discovery and selection update `C:\ProgramData\OSDeployCore\cache\osdeploymdt\config.json` with the discovered shares and selected drive. This happens before the function calls `ShouldProcess`, including in audit and `-WhatIf` runs. Stale persistent drives can also be removed before `ShouldProcess`.

## Audit and Initialization

After selection, the function calls `ShouldProcess` once for the selected deployment-share root. Without `-Force`, it reports work as `[OK]` or `[SKIP]`; with `-Force`, it performs the following actions.

### Folders and copied content

The function creates these folders when missing:

```
DEPLOYROOT\Templates
DEPLOYROOT\Templates\winpe-drivers
DEPLOYROOT\Templates\winpe-extrafiles
```

It copies these files from the MDT installation and overwrites the destinations:

| Source                                     | Destination                                |
| ------------------------------------------ | ------------------------------------------ |
| `INSTALLDIR\Templates\winpeshl.ini`        | `DEPLOYROOT\Templates\winpeshl.ini`        |
| `INSTALLDIR\Templates\Wimscript.ini`       | `DEPLOYROOT\Templates\wimscript.ini`       |
| `INSTALLDIR\Templates\Unattend_PE_x64.xml` | `DEPLOYROOT\Templates\Unattend_PE_x64.xml` |
| `INSTALLDIR\Templates\LiteTouchPE.xml`     | `DEPLOYROOT\Templates\LiteTouchPE.xml`     |
| `INSTALLDIR\Samples\Background.bmp`        | `DEPLOYROOT\Templates\background.bmp`      |

Missing source files produce warnings, but the function continues with later items.

### LiteTouchPE template

In `Templates\LiteTouchPE.xml`, the function changes exact matching copy sources for `Unattend_PE_%PLATFORM%.xml` and `winpeshl.ini` from `%INSTALLDIR%` to `%DEPLOYROOT%`.

It then configures the `WindowsPE/Components` node:

* If `Boot\LiteTouchPE_x64.xml` exists and contains a Components node, copy that complete node into the template.
* If the boot XML does not exist, replace the template's component children with the built-in OSDeploy list of WinPE components.
* If required XML nodes or exact source values are absent, leave those portions unchanged or warn and continue.

The function also adds this exit command when an exact matching entry does not already exist:

```
start /wait pwsh.exe -ExecutionPolicy Bypass -Command "Invoke-OSDeployMDT"
```

This command causes MDT to invoke OSDeploy at its LiteTouchPE exit stages during later **Update Deployment Share** operations.

### Deployment-share settings

When `Control\Settings.xml` exists, the function creates `Control\Settings.xml.backup` only if that backup does not already exist. It then changes exact matching values to:

* Use `%DEPLOYROOT%\Templates\background.bmp` for the x64 background.
* Set an empty `Boot.x64.ExtraDirectory` element to `%DEPLOYROOT%\Templates\winpe-extrafiles` without replacing an existing nonempty value.
* Set x64 scratch space from `32` to `512` MB.
* Change supported x86 settings from `True` to `False`, including x86 support, driver inclusion, boot WIM use, and LiteTouch ISO generation.

If `Settings.xml` is absent, the function reports that condition and completes. Values that do not exactly match the expected source text are not normalized.

## WhatIf and Confirmation

The function uses one `ShouldProcess` call after MDT discovery and share selection but before the share audit body. Therefore:

* `-WhatIf` does not produce the detailed `[OK]` and `[SKIP]` audit.
* `-Confirm` prompts once for the selected deployment-share root.
* Declining confirmation returns without auditing or changing the selected share.
* Discovery can still write the OSDeploy MDT cache and prune stale persistent drives before either boundary.

`-Force` controls writes inside the body; it does not bypass `-WhatIf` or `-Confirm`.

## Exit and Failure Behavior

Blocking host checks throw terminating errors. An unresolved MDT installation, no reachable deployment share, invalid selection, cancellation, or declined confirmation writes a warning or confirmation result and returns without entering the audit body.

Individual missing source files and XML update failures generally warn and allow remaining operations to continue. A forced run can therefore complete only part of the initialization. Review warnings and rerun the audit afterward.

## Output

The function intentionally returns no pipeline object. Audit status, warnings, and completion guidance are written to the host or warning streams. Selection details are available with `-Verbose`.

After initialization, run **Update Deployment Share** to execute OSDeploy customization. See the [MDT installation workflow](../../archive-osdeploymdt/install-osdeploymdt.md), the [Invoke-OSDeployMDT guide](invoke-osdeploymdt.md), or the [Install-OSDeployMDT command reference](../../command-reference/osdeploy/install-osdeploymdt.md).

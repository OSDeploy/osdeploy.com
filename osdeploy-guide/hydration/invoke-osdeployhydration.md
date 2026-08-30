---
description: >-
  Prepare an OSDeploy workstation, refresh Windows and driver content, build
  Hydra boot media, and optionally test it in Hyper-V.
icon: rectangle-terminal
---

# Invoke-OSDeployHydration

`Invoke-OSDeployHydration` coordinates the complete OSDeploy workstation setup workflow. It installs required and optional software, downloads and imports Windows content, refreshes WinPE drivers, builds Hydra boot media, and can create a Hyper-V test VM on a physical host.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or later. PowerShell must be installed from the MSI package, and `curl.exe` must be available in `PATH`.

Install the [OSDeploy module](../requirements/powershell-modules/) and OSDCloud module version `26.5.24.1` or later before starting hydration:

```powershell
Install-Module -Name OSDeploy -Force -SkipPublisherCheck
Install-Module -Name OSDCloud -Force -SkipPublisherCheck
```

{% hint style="warning" %}
The function stops before hydration when the Windows version, PowerShell installation, `curl.exe`, administrator access, or OSDCloud requirement is not met. It does not install or upgrade these prerequisites.
{% endhint %}

Hydration can install the remaining workstation components:

| Component                         | Requirement                   | Behavior when absent                                   |
| --------------------------------- | ----------------------------- | ------------------------------------------------------ |
| Windows ADK 25H2 and WinPE add-on | Required                      | Prompt to install. Declining terminates hydration.     |
| 7-Zip                             | Required                      | Prompt to install. Declining terminates hydration.     |
| Git for Windows                   | Optional                      | Prompt to install. Declining skips Git and continues.  |
| Visual Studio Code                | Optional                      | Prompt to install. Declining skips it and continues.   |
| Visual Studio Code Insiders       | Optional                      | Prompt to install. Declining skips it and continues.   |
| Hyper-V                           | Optional, physical hosts only | Prompt to enable it. Declining skips it and continues. |

{% hint style="warning" %}
Enabling Hyper-V can require a restart. Hydration does not restart the workstation. Restart before creating a test VM when Hyper-V is pending a restart.
{% endhint %}

## Parameters

`Invoke-OSDeployHydration` has one function-specific parameter and supports `-WhatIf` and `-Confirm` through `SupportsShouldProcess`.

| Parameter  | Type             | Default     | Accepted values and behavior                                                                                                                                                                                                                                                   |
| ---------- | ---------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-Force`   | `Switch`         | Not enabled | Bypass hydration-level workflow, component-installation, and Hyper-V test prompts. Force ESD and driver refresh behavior. It is not forwarded as a parameter to Windows import, boot-media build, or VM creation.                                                              |
| `-WhatIf`  | Common parameter | Not enabled | Decline the parent ESD/import operation and preview `ShouldProcess` operations in nested installer, driver, boot-build, and VM commands through the inherited PowerShell preference. It does not suppress `ShouldContinue` prompts or pre-gate discovery and profile handling. |
| `-Confirm` | Common parameter | Not enabled | Request confirmation for `ShouldProcess` operations. It does not replace or suppress the separate hydration-level `ShouldContinue` prompts.                                                                                                                                    |

The function does not accept an architecture parameter. It uses `arm64` only when `PROCESSOR_ARCHITECTURE` is `ARM64`; every other value resolves to `amd64`.

## Examples

### Run interactive hydration

Review the complete workflow and decide whether to install each missing component and create the optional Hyper-V test VM:

```powershell
Invoke-OSDeployHydration
```

Hydration prompts separately for the overall workflow and each missing component. Required ADK or 7-Zip installation cannot be skipped if the workflow is to continue.

### Refresh content and accept hydration prompts

Install every missing component, force ESD and driver refresh behavior, and accept the Hyper-V test on a supported physical host:

```powershell
Invoke-OSDeployHydration -Force
```

`-Force` does not make every delegated command non-interactive. ESD cache or download decisions can still prompt, and `Build-OSDeployBoot -Auto` can still show shared content and wallpaper selectors.

### Preview hydration without the workflow prompts

Use the inherited PowerShell `WhatIf` preference for nested commands while bypassing hydration-level `ShouldContinue` prompts:

```powershell
Invoke-OSDeployHydration -Force -WhatIf
```

This preview can inspect installed software, cached content, vendor catalogs, source images, and build profiles. It does not provide a side-effect-free simulation; see [WhatIf Behavior](invoke-osdeployhydration.md#whatif-behavior) before running it.

### Show detailed selection information

Display architecture detection, skipped optional actions, catalog processing, and child-command details:

```powershell
Invoke-OSDeployHydration -Verbose
```

Verbose output is especially useful when a cached ESD, existing import, unavailable driver source, disabled Hyper-V feature, or missing ISO causes a stage to be skipped.

### Capture child-command results

Collect all objects written to the success stream by the delegated commands:

```powershell
$results = @(Invoke-OSDeployHydration)

$results | Group-Object { $_.GetType().FullName } |
	Select-Object Name, Count
```

The collection can contain several object types. `Invoke-OSDeployHydration` does not combine them into one summary object.

## Workflow Order

Hydration runs these stages in order:

1. Validate Windows, PowerShell, `curl.exe`, elevation, and OSDCloud.
2. Initialize the OSDeploy Core directory structure and detect the host architecture.
3. Display the planned workflow and request overall confirmation unless `-Force` is used.
4. Test and optionally install required and recommended workstation software.
5. Download and import the Windows Enterprise ESD when the parent `ShouldProcess` operation is approved.
6. Refresh WinPE driver packages for the detected architecture.
7. Run `Build-OSDeployBoot -Name 'Hydra' -Auto`.
8. On a physical host, offer to create a Hyper-V VM when Hyper-V is enabled and the new `bootmedia.iso` exists.

A terminating error stops later stages. Non-terminating warnings from a child command can allow the workflow to continue with cached content or skip only the affected source.

## Software Installation

Hydration tests each component before prompting. Existing components are retained; no installation action runs for a component whose test already succeeds.

For missing required software, accepting the prompt calls:

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -Force
Install-OSDeploySoftware -Name '7zip' -Force
```

Declining either installation throws a terminating error. The ADK is required to build media even when the eventual source is an imported WinRE image.

Missing Git, Visual Studio Code, and Visual Studio Code Insiders are optional. Hydration calls the Git installer with `-NonInteractive`, which retains existing global Git identity values without asking whether to change them. In the current implementation, missing `user.email` or `user.name` values still produce `Read-Host` prompts.

Hyper-V installation is offered only when `Test-IsVM` does not identify the workstation as a virtual machine. Hydration skips both Hyper-V installation and VM testing when it detects Hyper-V, VMware, VirtualBox, or QEMU virtualization markers.

## Windows Content

The Windows content stage is gated as one high-impact operation named `Download and import Windows ESD`. When approved, it runs:

```powershell
Update-OSDeployCoreESD -Architecture $arch
Update-OSDeployCoreOS -Architecture $arch
```

`-Force`, `-WhatIf`, and `-Confirm` are forwarded to `Update-OSDeployCoreESD` through bound parameters. The Windows import receives inherited common-parameter preferences but does not receive `-Force`.

The ESD command selects the current en-US Windows 11 25H2 Enterprise ESD for the detected architecture, checks cached files and download availability, downloads when required, and verifies the SHA256 checksum. `-Force` requests a refresh even when a verified current file exists. ESD cache and recovery decisions can still require confirmation.

Verified ESD files are stored under:

```
C:\ProgramData\OSDeployCore\OSDCloud\OS\Windows 11 25H2\
```

The import command creates Windows OS and Windows RE content under architecture-specific directories below:

```
C:\ProgramData\OSDeployCore\cache\windows-os\
C:\ProgramData\OSDeployCore\cache\windows-re\
```

When both matching import directories already exist, `Update-OSDeployCoreOS` skips that ESD. When only one exists, it processes the ESD again to complete the pair.

## WinPE Drivers

Hydration runs `Update-OSDeployCoreDrivers` for the detected architecture without specifying `-Name`, so every active matching source is processed. Current AMD64 sources include Dell, HP, Intel Ethernet, and Intel Wi-Fi packages. An ARM64 run can find no matching vendor packages when the installed OSDeploy configuration contains no active ARM64 sources.

The driver command discovers current package metadata, updates its local catalog, downloads and verifies matching archives, and expands them into the OSDeploy driver library. A source failure writes a warning and does not prevent later sources from being processed.

Downloads and expanded content are stored below:

```
C:\ProgramData\OSDeployCore\cache\downloads\
C:\ProgramData\OSDeployCore\OSDRepo\winpe-drivers\
```

When no imported Windows source exists, the driver command automatically skips Wi-Fi packages because wireless drivers apply to WinRE-based boot images, not ADK WinPE.

## Hydra Boot Media

Hydration calls the boot builder with a fixed name and automatic source selection:

```powershell
Build-OSDeployBoot -Name 'Hydra' -Auto
```

`Build-OSDeployBoot` derives the architecture from the host, selects the newest imported WinRE source for that architecture, and falls back to the architecture-specific ADK `winpe.wim` when no WinRE source is available.

`-Auto` skips the saved-profile picker but does not suppress every selector. Shared drivers, WinPE applications, WinPE scripts, media scripts, WinPEStartup profiles, and wallpaper can still require selection. The builder writes or updates the architecture-specific `Hydra` profile and discovers profile-local content before reaching its build-directory confirmation.

Completed media is written below:

```
C:\ProgramData\OSDeployCore\boot\{Windows build}.{revision}-{architecture}-Hydra\
```

If that directory exists, the builder adds `-001`, `-002`, or a later suffix instead of overwriting it. The standard ISO is `bootmedia.iso` in the final build directory.

See [Build-OSDeployBoot](../osdeploy-boot/build-osdeployboot.md) for source, profile, content, and media behavior.

## Hyper-V Test

Hydration evaluates the VM test only on a physical host. It constructs the ISO path from `$global:BuildMedia.MediaRootPath` after the boot build and requires both of these conditions:

* Hyper-V is enabled.
* `bootmedia.iso` exists in the completed Hydra build directory.

When both conditions are met, hydration prompts before running:

```powershell
New-OSDeployHyperVM -ISO $isoPath
```

The VM command uses its own defaults for name, generation, memory, processors, disk, switch, checkpoint, and startup. Hydration does not pass `-Force` or explicit VM settings. With `-Force`, hydration accepts the test prompt, but VM creation still depends on the child command's requirements and `ShouldProcess` behavior.

The test is skipped when the workstation is a VM, Hyper-V is not enabled, or the ISO does not exist. Use `-Verbose` to distinguish the Hyper-V and ISO conditions on a physical host.

See [New-OSDeployHyperVM](../osdeploy-hypervm/new-osdeployhypervm.md) for VM defaults and host requirements.

## Force Behavior

`-Force` has a specific scope:

| Operation        | Effect of `-Force`                                                                                   |
| ---------------- | ---------------------------------------------------------------------------------------------------- |
| Overall workflow | Bypass the hydration-level confirmation.                                                             |
| Missing software | Accept every required and optional installation prompt.                                              |
| ESD update       | Forward `-Force` to refresh selected ESD content. Child cache and recovery prompts can still appear. |
| Windows import   | No `-Force` parameter is passed. Existing complete imports remain skipped.                           |
| Driver update    | Forward `-Force` to refresh catalog entries and matching packages.                                   |
| Boot build       | No `-Force` parameter exists or is passed. Build content selectors can still appear.                 |
| Hyper-V test     | Accept the hydration-level VM test prompt. No `-Force` parameter is passed to VM creation.           |

Use `-Force` when all optional workstation tools and the VM test are wanted. Do not use it merely to refresh Windows or drivers if the optional installations are not desired.

## WhatIf Behavior

PowerShell sets `$WhatIfPreference` in the hydration scope, and nested advanced functions inherit that preference even when `-WhatIf` is not explicitly present in a child command line.

With `-WhatIf`:

* Hydration still performs prerequisite checks, architecture detection, installed-software tests, and `ShouldContinue` prompts.
* Installer commands that support `ShouldProcess` preview their installation actions instead of approving them.
* The parent Windows ESD/import gate is declined, so neither child command runs in that stage.
* The driver command can still contact vendor sources and evaluate catalogs, but its download and expansion actions are not approved.
* `Build-OSDeployBoot` still performs source selection, profile handling, content discovery, configuration display, and its five-second delay before its inherited `WhatIf` preference declines build-directory creation.
* The Hyper-V test normally has no new ISO to use and is skipped. If VM creation is reached, `New-OSDeployHyperVM` inherits the same `WhatIf` preference.

{% hint style="warning" %}
`-WhatIf` is not side-effect free. Initialization and discovery can occur before child `ShouldProcess` gates. In particular, `Build-OSDeployBoot` can create or update a Hydra build profile, convert module paths to portable tokens, and copy a selected wallpaper before stopping. If the ADK or 7-Zip is absent, previewed installation does not satisfy that requirement, and a later child command can stop or return before completing its preview.
{% endhint %}

Combine `-Force` and `-WhatIf` to bypass hydration-level `ShouldContinue` prompts during a preview. `-Confirm:$false` suppresses `ShouldProcess` confirmation but does not bypass `ShouldContinue` prompts.

## Output

`Invoke-OSDeployHydration` does not create a single hydration result. It passes through success-stream output from the child commands that run.

| Type                                          | Source and behavior                                                                                                                  |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `System.Management.Automation.PSCustomObject` | Software installation status objects and, when created, the Hyper-V VM result. Properties depend on the emitting child command.      |
| `System.IO.FileInfo`                          | Verified ESD files and successfully processed driver package files. An unavailable, declined, or failed item returns no file object. |
| `System.IO.DirectoryInfo`                     | Newly imported Windows OS directories. An existing complete import is skipped and does not return another directory object.          |
| `System.String`                               | Native installer output can reach the success stream during delegated software installation.                                         |

`Build-OSDeployBoot` does not emit a pipeline object. It populates `$global:BuildMedia`, which hydration uses to locate the generated ISO for the optional VM test.

The exact number and order of returned objects depend on installed software, cache state, available catalogs, accepted prompts, architecture, build success, and Hyper-V availability. Wrap the invocation in `@(...)` when a consistently array-shaped result is required.

See [Hydration](./) for the workflow overview or the [Invoke-OSDeployHydration command reference](../../command-reference/osdeploy/invoke-osdeployhydration.md) for compact syntax and parameter definitions.

---
description: >-
  GitBook Assistant Agent instructions for planning, previewing, and creating a
  Hyper-V virtual machine with New-OSDeployHyperVM.
icon: brackets-curly
---

# New-OSDeployHyperVM Skill

Use these instructions to help a user create a Hyper-V virtual machine for testing OSDeploy or OSDCloud boot media.

{% hint style="info" %}
Prefer the function defaults and automatic discovery. Add a parameter only when the user requests a different value or the environment requires an explicit choice.
{% endhint %}

## Agent contract

Build and, when requested, run a valid `New-OSDeployHyperVM` command. Treat VM creation as a mutating operation.

Follow these rules:

* Run the command only from an elevated PowerShell 7.6 or later session on a physical Windows 11 25H2 build 26200 or later host.
* Confirm that the current OSDeploy module exports `New-OSDeployHyperVM` before planning execution.
* Use `-WhatIf` first when the user asks to create a VM. Show the resolved plan and obtain approval before running the command without `-WhatIf`.
* Do not run the creation command when the user asks only for syntax, an example, a recommendation, or a preview.
* Do not install Hyper-V, restart Windows, delete an existing VM or VHDX, or change host networking unless the user explicitly requests that separate operation.
* Do not invent parameter names or pass values outside the validation rules in this skill.
* Omit `-ISO` to use automatic OSDeploy ISO discovery. Do not guess a path.
* Omit `-SwitchName` to use automatic switch selection. Do not create a virtual switch as part of this workflow.
* Use explicit PowerShell Boolean values for `-CheckpointVM` and `-StartVM`, such as `-StartVM $false`.
* Capture and inspect the returned object when executing the function.

## Requirements

Confirm these conditions before execution:

| Requirement | Required state                                                           |
| ----------- | ------------------------------------------------------------------------ |
| Host        | Physical Windows 11 25H2 build 26200 or later computer                   |
| PowerShell  | Elevated PowerShell 7.6 or later installed from the MSI package          |
| Module      | Current OSDeploy module with `New-OSDeployHyperVM` exported              |
| Hyper-V     | Hyper-V platform and PowerShell commands enabled with no pending restart |
| Utility     | `curl.exe` available in `PATH`                                           |
| Capacity    | Enough host memory, processors, and storage for the requested VM         |

The function checks these requirements and stops before VM creation when a required condition is missing. It does not support nested virtualization.

Use this non-mutating preflight when the user asks to validate the host:

```powershell
$preflight = [ordered]@{
	WindowsVersion = [System.Environment]::OSVersion.VersionString
	PowerShellVersion = $PSVersionTable.PSVersion.ToString()
	Elevated = ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole(
		[Security.Principal.WindowsBuiltInRole]::Administrator
	)
	CommandAvailable = [bool](Get-Command -Name 'New-OSDeployHyperVM' -ErrorAction SilentlyContinue)
	HyperVCommandsAvailable = [bool](Get-Command -Name 'New-VM' -ErrorAction SilentlyContinue)
	CurlAvailable = [bool](Get-Command -Name 'curl.exe' -ErrorAction SilentlyContinue)
}

[pscustomobject]$preflight
```

{% hint style="warning" %}
Do not infer that the host is ready from the preflight table alone. The function performs additional Windows build, MSI installation, physical-host, Hyper-V, and pending-restart checks during `-WhatIf` processing.
{% endhint %}

## Collect the request

Use the defaults when the user does not specify a preference. Ask only for information needed to resolve an explicit requirement.

| User requirement                     | Parameter decision                                                 |
| ------------------------------------ | ------------------------------------------------------------------ |
| Use the newest OSDeploy boot image   | Omit `-ISO`                                                        |
| Use a specific boot image            | Set `-ISO` to an existing `.iso` file                              |
| Use standard Windows boot media      | Keep Generation 2 and `MicrosoftWindows`                           |
| Use Linux or other signed media      | Ask which supported Secure Boot template the media requires        |
| Inspect or modify the VM before boot | Set `-StartVM $false`                                              |
| Avoid the initial checkpoint         | Set `-CheckpointVM $false`                                         |
| Use a specific network               | Set `-SwitchName` to an existing switch returned by `Get-VMSwitch` |
| Leave networking automatic           | Omit `-SwitchName`                                                 |
| Use recommended OSDeploy memory      | Set `-MemoryStartupGB 12` when host capacity permits               |

If the user supplies a relative ISO path, resolve it to an existing file before building the command. If the path does not exist or does not end in `.iso`, stop and request a valid path.

## Parameter reference

All parameters are optional.

| Parameter             | Type             | Default            | Accepted values and behavior                                                                                                                             |
| --------------------- | ---------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-ISO`                | `String`         | Automatic          | Existing `.iso` file. When omitted, select the newest `bootmedia.iso` under `C:\ProgramData\OSDeployCore\boot`; use an empty DVD drive when none exists. |
| `-NamePrefix`         | `String`         | `OSDeploy`         | Text appended to the `yyMMdd-HHmmss` timestamp in the VM name.                                                                                           |
| `-Generation`         | `UInt16`         | `2`                | `1` or `2`. Generation 2 receives Secure Boot and optional virtual TPM configuration.                                                                    |
| `-MemoryStartupGB`    | `UInt16`         | `8`                | Fixed startup memory from 2 through 64 GB. Use 12 GB when requested and host capacity permits.                                                           |
| `-ProcessorCount`     | `UInt16`         | `2`                | From 1 through 64 virtual processors.                                                                                                                    |
| `-VHDSizeGB`          | `UInt16`         | `64`               | New VHDX size from 8 through 512 GB.                                                                                                                     |
| `-DisplayResolution`  | `String`         | `1600x900`         | One value from the supported resolution list.                                                                                                            |
| `-SwitchName`         | `String`         | Automatic          | Existing Hyper-V switch. When omitted, select `Default Switch`, then the first available switch, then no switch.                                         |
| `-SecureBootTemplate` | `String`         | `MicrosoftWindows` | `MicrosoftWindows`, `MicrosoftUEFICertificateAuthority`, or `OpenSourceShieldedVM`. Generation 1 ignores the value.                                      |
| `-CheckpointVM`       | `Boolean`        | `$true`            | Create the `New-OSDeployHyperVM` checkpoint before startup.                                                                                              |
| `-StartVM`            | `Boolean`        | `$true`            | Open VMConnect when available, wait 10 seconds, and start the VM.                                                                                        |
| `-WhatIf`             | Common parameter | Not enabled        | Resolve the plan and return it without creating the VM.                                                                                                  |
| `-Confirm`            | Common parameter | Not enabled        | Request confirmation before VM creation and configuration.                                                                                               |

Supported display resolutions:

```
640x480, 800x600, 1024x768, 1152x864, 1280x720, 1280x768,
1280x800, 1280x960, 1280x1024, 1360x768, 1366x768, 1400x1050,
1440x900, 1600x900, 1680x1050, 1920x1080, 1920x1200,
2560x1440, 2560x1600, 3840x2160, 3840x2400, 4096x2160
```

## Build the command

Start with the smallest valid command:

```powershell
New-OSDeployHyperVM
```

Add only the parameters required by the request. Use splatting when an agent is executing a customized command because it keeps values typed and reviewable:

```powershell
$vmParameters = @{
	NamePrefix = 'OSDeploy Lab'
	MemoryStartupGB = 12
	ProcessorCount = 2
	VHDSizeGB = 128
	StartVM = $false
}

$plan = New-OSDeployHyperVM @vmParameters -WhatIf
$plan | Format-List
```

Do not add an automatic value to the splat. For example, omit `ISO` instead of searching for and injecting the newest OSDeploy ISO yourself. This preserves the function's discovery and fallback behavior.

## Preview and approval workflow

{% stepper %}
{% step %}
### Validate explicit values

Check the ISO path, numeric ranges, generation, display resolution, Secure Boot template, and Boolean syntax. When a switch is explicit, confirm that `Get-VMSwitch -Name '<name>'` resolves it.
{% endstep %}

{% step %}
### Run the preview

Invoke the complete command with `-WhatIf` and capture the result. The function still performs prerequisite checks, ISO discovery, switch selection, and VM host path resolution.
{% endstep %}

{% step %}
### Present the resolved plan

Report `VMName`, `ISOPath`, `VHDPath`, `SwitchName`, `Generation`, `MemoryStartupGB`, `ProcessorCount`, and `VHDSizeGB`. Clearly identify an empty `ISOPath` or `SwitchName`.
{% endstep %}

{% step %}
### Obtain approval

Ask for confirmation before removing `-WhatIf`. Approval applies to the exact previewed parameters. Preview again when a parameter changes.
{% endstep %}

{% step %}
### Create and verify the VM

Run the approved command without `-WhatIf`, capture the result, and inspect the status properties. Do not retry automatically after a partial failure because the VM or VHDX might already exist.
{% endstep %}
{% endstepper %}

## Execution examples

### Preview the defaults

Resolve automatic ISO and switch selection without creating a VM:

```powershell
$plan = New-OSDeployHyperVM -WhatIf
$plan | Format-List
```

### Preview an explicit ISO

Use an existing boot image and leave the proposed VM stopped:

```powershell
$plan = New-OSDeployHyperVM `
	-ISO 'D:\ISO\OSDeployBoot.iso' `
	-MemoryStartupGB 12 `
	-StartVM $false `
	-WhatIf

$plan | Format-List
```

### Create an approved VM

After the user approves the exact preview, run the same parameters without `-WhatIf`:

```powershell
$result = New-OSDeployHyperVM `
	-ISO 'D:\ISO\OSDeployBoot.iso' `
	-MemoryStartupGB 12 `
	-StartVM $false

$result | Format-List
```

## Automatic behavior

When `-ISO` is omitted, the function recursively searches `C:\ProgramData\OSDeployCore\boot` for `bootmedia.iso`, sorts matches by last-modified time, and uses the newest file. No match is not an error; the new DVD drive is empty.

When `-SwitchName` is omitted, the function uses `Default Switch`, the first switch returned by `Get-VMSwitch`, or no switch in that order. An explicit switch name is passed to Hyper-V and creation stops if it cannot be resolved.

Generation 2 sets the DVD drive as the first boot device and enables Secure Boot. When the physical TPM is present and ready, the function applies the available Hyper-V security commands and enables a virtual TPM. Generation 1 skips these actions.

The VHDX is created in the Hyper-V host's default virtual hard disk directory. The function disables dynamic memory and automatic checkpoints, configures the requested resources and display, optionally creates an initial checkpoint, optionally opens VMConnect, and then starts the VM.

## Verify the result

A successful command returns a `System.Management.Automation.PSCustomObject`. Inspect these properties:

| Property            | Verification                                                     |
| ------------------- | ---------------------------------------------------------------- |
| `VMName`            | Final timestamped VM name                                        |
| `ISOPath`           | Mounted ISO, or `$null` for an empty DVD drive                   |
| `VHDPath`           | Path to the new VHDX                                             |
| `SwitchName`        | Selected switch, or empty for an unconnected VM                  |
| `Generation`        | Requested generation                                             |
| `MemoryStartupGB`   | Requested fixed startup memory                                   |
| `ProcessorCount`    | Requested virtual processor count                                |
| `VHDSizeGB`         | Requested VHDX size                                              |
| `DisplayResolution` | Configured resolution; omitted from a `-WhatIf` result           |
| `Created`           | `$true` only after successful creation                           |
| `Started`           | `$true` only when this invocation started the VM                 |
| `Checkpointed`      | `$true` only when this invocation created the initial checkpoint |
| `StartVMConnect`    | `$true` only when this invocation launched VMConnect             |

For a preview, expect `Created`, `Started`, `Checkpointed`, and `StartVMConnect` to be `$false`. For an actual run, compare the status values with the approved options. An empty `ISOPath` or `SwitchName` is a valid fallback, but report it because it affects boot or network availability.

## Handle failures

Report the exact terminating error and the last confirmed stage. Do not automatically rerun the function after `New-VM` might have executed. First inspect for a partially created VM and VHDX:

```powershell
Get-VM -Name '<planned VM name>' -ErrorAction SilentlyContinue
Test-Path -LiteralPath '<planned VHDX path>'
```

Do not remove either object without explicit user approval. For prerequisite failures, direct the user to [Create a Hyper-V VM](./). For complete behavior and parameter examples, use the [detailed New-OSDeployHyperVM guide](new-osdeployhypervm.md) or the [command reference](../../command-reference/osdeploy/new-osdeployhypervm.md).

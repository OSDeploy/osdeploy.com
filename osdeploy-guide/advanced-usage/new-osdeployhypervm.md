---
description: Configure and create an OSDeploy Hyper-V virtual machine.
---

# New-OSDeployHyperVM

`New-OSDeployHyperVM` creates a Hyper-V virtual machine for testing OSDeploy and OSDCloud boot media. Use its parameters to control the mounted ISO, VM name, generation, resources, display, networking, firmware, checkpoint, and startup behavior.

## Requirements

Run the function from an elevated PowerShell 7.6 or later session on a physical Windows 11 25H2 build 26200 or later host. PowerShell must be installed from the MSI package, `curl.exe` must be available in `PATH`, and Hyper-V and its PowerShell tools must be enabled.

See [Module Setup](../requirements/powershell-modules/) to install OSDeploy and [Microsoft Hyper-V](install-osdeploysoftware/microsoft-hyper-v.md) to enable Hyper-V.

{% hint style="warning" %}
Restart the host after enabling Hyper-V. The function stops when Hyper-V is pending a restart. Nested virtualization is not supported because the function must run on a physical host.
{% endhint %}

## Resource Guidance

The function defaults to 2 virtual processors and 8 GB of fixed startup memory. OSDeploy recommends at least 2 virtual processors and 12 GB of startup memory for smoother deployment work when the host has enough resources.

| Resource           | Function default | OSDeploy recommendation                     |
| ------------------ | ---------------- | ------------------------------------------- |
| Virtual processors | 2                | At least 2                                  |
| Startup memory     | 8 GB             | At least 12 GB when host capacity permits   |
| Virtual disk       | 64 GB            | 64 GB or more when the workflow requires it |

{% hint style="warning" %}
Do not allocate resources needed by the physical host. Reduce VM memory or processor count on a constrained computer, and make sure the Hyper-V virtual disk location has enough free space.
{% endhint %}

## Parameters

All parameters are optional.

| Parameter             | Type             | Default            | Accepted values and behavior                                                                                                                |
| --------------------- | ---------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `-ISO`                | `String`         | Automatic          | Use an existing `.iso` file. When omitted, the function selects the newest `bootmedia.iso` under `C:\ProgramData\OSDeployCore\boot`.        |
| `-NamePrefix`         | `String`         | `OSDeploy`         | Append text to the timestamp at the beginning of the VM name.                                                                               |
| `-Generation`         | `UInt16`         | `2`                | Use `1` or `2`. Secure Boot and virtual TPM configuration apply only to Generation 2.                                                       |
| `-MemoryStartupGB`    | `UInt16`         | `8`                | Use a fixed startup-memory value from 2 through 64 GB.                                                                                      |
| `-ProcessorCount`     | `UInt16`         | `2`                | Use from 1 through 64 virtual processors.                                                                                                   |
| `-VHDSizeGB`          | `UInt16`         | `64`               | Create a VHDX from 8 through 512 GB.                                                                                                        |
| `-DisplayResolution`  | `String`         | `1600x900`         | Use one of the validated Hyper-V display resolutions from `640x480` through `4096x2160`.                                                    |
| `-SwitchName`         | `String`         | Automatic          | Use an existing Hyper-V virtual switch. When omitted, prefer `Default Switch`, then the first available switch, then no switch.             |
| `-SecureBootTemplate` | `String`         | `MicrosoftWindows` | Use `MicrosoftWindows`, `MicrosoftUEFICertificateAuthority`, or `OpenSourceShieldedVM` for Generation 2. Generation 1 ignores this setting. |
| `-CheckpointVM`       | `Boolean`        | `$true`            | Create the initial `New-OSDeployHyperVM` checkpoint.                                                                                        |
| `-StartVM`            | `Boolean`        | `$true`            | Open VMConnect when available and start the VM.                                                                                             |
| `-WhatIf`             | Common parameter | Not enabled        | Return the planned configuration without creating the VM.                                                                                   |
| `-Confirm`            | Common parameter | Not enabled        | Prompt before creating and configuring the VM.                                                                                              |

## Examples

### Create a VM with the defaults

Create a Generation 2 VM with 2 virtual processors, 8 GB of memory, and a 64 GB VHDX. The function selects the newest OSDeploy `bootmedia.iso`, creates a checkpoint, opens VMConnect when available, and starts the VM.

```powershell
New-OSDeployHyperVM
```

### Use the recommended memory

Create a VM with 2 virtual processors and 12 GB of startup memory when the host has enough capacity:

```powershell
New-OSDeployHyperVM -ProcessorCount 2 -MemoryStartupGB 12
```

### Mount a specific ISO

Mount an existing ISO instead of searching OSDeployCore:

```powershell
New-OSDeployHyperVM -ISO 'D:\ISO\OSDeployBoot.iso'
```

The path must identify an existing file with an `.iso` extension.

### Customize the name and resources

Create a VM whose timestamped name ends with `OSDCloud Lab`. Allocate 4 virtual processors, 16 GB of startup memory, and a 128 GB VHDX:

```powershell
New-OSDeployHyperVM `
	-NamePrefix 'OSDCloud Lab' `
	-ProcessorCount 4 `
	-MemoryStartupGB 16 `
	-VHDSizeGB 128
```

### Create the VM without starting it

Create and checkpoint the VM, but leave it turned off. Use this option to review its settings or attach additional hardware before the first boot:

```powershell
New-OSDeployHyperVM -StartVM $false
```

### Skip the initial checkpoint

Create and start the VM without creating the `New-OSDeployHyperVM` checkpoint:

```powershell
New-OSDeployHyperVM -CheckpointVM $false
```

### Create a Generation 1 VM

Create a Generation 1 VM with 4 GB of memory and leave it turned off:

```powershell
New-OSDeployHyperVM `
	-Generation 1 `
	-MemoryStartupGB 4 `
	-StartVM $false
```

Generation 1 does not use the `-SecureBootTemplate` setting and does not receive the Generation 2 firmware or virtual TPM configuration.

### Select a virtual switch

Connect the VM to an existing switch instead of using automatic switch selection:

```powershell
New-OSDeployHyperVM -SwitchName 'OSDeploy External'
```

Use PowerShell argument completion for `-SwitchName`, or list available switches before creating the VM:

```powershell
Get-VMSwitch
```

### Select a Secure Boot template

Create a Generation 2 VM that uses the Microsoft UEFI Certificate Authority template:

```powershell
New-OSDeployHyperVM `
	-Generation 2 `
	-SecureBootTemplate 'MicrosoftUEFICertificateAuthority'
```

Use `MicrosoftWindows` for standard Windows boot media. Use another template only when the selected boot media requires it.

### Change the display resolution

Configure the Hyper-V video adapter for a single `1920x1080` resolution:

```powershell
New-OSDeployHyperVM -DisplayResolution '1920x1080'
```

Supported values are:

```
640x480, 800x600, 1024x768, 1152x864, 1280x720, 1280x768,
1280x800, 1280x960, 1280x1024, 1360x768, 1366x768, 1400x1050,
1440x900, 1600x900, 1680x1050, 1920x1080, 1920x1200,
2560x1440, 2560x1600, 3840x2160, 3840x2400, 4096x2160
```

### Preview the configuration

Use `-WhatIf` to resolve the ISO, switch, VM name, and VHDX path without creating the VM:

```powershell
New-OSDeployHyperVM -MemoryStartupGB 12 -WhatIf
```

The returned object describes the planned VM and sets `Created`, `Started`, `Checkpointed`, and `StartVMConnect` to `$false`.

### Capture the result

Save the returned object and inspect the VM identity and action status:

```powershell
$result = New-OSDeployHyperVM `
	-ISO 'D:\ISO\OSDeployBoot.iso' `
	-MemoryStartupGB 12 `
	-StartVM $false

$result | Format-List
```

### Create a fully customized VM

Combine parameters to create a stopped Generation 2 lab VM with explicit media, networking, resources, firmware, and display settings:

```powershell
New-OSDeployHyperVM `
	-ISO 'D:\ISO\OSDeployBoot.iso' `
	-NamePrefix 'OSDeploy Lab' `
	-Generation 2 `
	-MemoryStartupGB 12 `
	-ProcessorCount 2 `
	-VHDSizeGB 128 `
	-DisplayResolution '1920x1080' `
	-SwitchName 'Default Switch' `
	-SecureBootTemplate 'MicrosoftWindows' `
	-CheckpointVM $true `
	-StartVM $false
```

## ISO Selection

When `-ISO` is omitted, the function searches `C:\ProgramData\OSDeployCore\boot` and its subdirectories for files named `bootmedia.iso`. It sorts matching files by their last-modified time and mounts the newest one.

When no matching ISO exists, the function creates an empty DVD drive. The VM is still created, but it has no OSDeploy media from which to boot. Attach an ISO before starting or restarting it.

Use `-Verbose` to display the automatically selected ISO or the empty-DVD fallback:

```powershell
New-OSDeployHyperVM -Verbose
```

## Network Selection

When `-SwitchName` is omitted, the function applies this precedence:

1. Use the switch named `Default Switch`.
2. Use the first switch returned by `Get-VMSwitch`.
3. Create the VM without a virtual switch when no switches exist.

When `-SwitchName` is specified, Hyper-V must be able to resolve that switch name. The function passes the value to `New-VM` and stops if Hyper-V cannot create the connection.

## Generation 2 Security

For a Generation 2 VM, the function sets the DVD drive as the first boot device and enables Secure Boot with the selected template.

The function checks the physical host TPM. When it is present and ready, the function applies each available Hyper-V security command to configure VM security, create a local key protector, and enable the virtual TPM. When the host TPM is absent or not ready, VM creation continues without those optional virtual TPM settings.

Generation 1 skips these firmware, Secure Boot, and virtual TPM actions.

## VM Configuration

The function also configures these settings:

* Creates the VHDX in the Hyper-V host's default virtual hard disk directory.
* Prefixes `-NamePrefix` with a `yyMMdd-HHmmss` timestamp.
* Disables dynamic memory and uses the specified fixed startup memory.
* Configures the display adapter for one resolution.
* Enables the Hyper-V Guest Service Interface when it is available.
* Disables Hyper-V automatic checkpoints.
* Sets the automatic start action to `Nothing`.
* Sets the automatic stop action to `Shutdown`.

The optional initial checkpoint is separate from Hyper-V automatic checkpoints. When `-CheckpointVM` is `$true`, the function creates a checkpoint named `New-OSDeployHyperVM` after configuration and before startup.

When `-StartVM` is `$true`, the function opens `vmconnect.exe` when it is available, waits 10 seconds, and then starts the VM. The VM still starts when VMConnect is unavailable.

## Output

After successful creation, the function returns a `System.Management.Automation.PSCustomObject` with these properties:

| Property            | Description                                               |
| ------------------- | --------------------------------------------------------- |
| `VMName`            | Final timestamped VM name.                                |
| `ISOPath`           | Mounted ISO path, or `$null` when the DVD drive is empty. |
| `VHDPath`           | Full path to the new VHDX.                                |
| `SwitchName`        | Selected switch name, or no value for an unconnected VM.  |
| `Generation`        | VM generation.                                            |
| `MemoryStartupGB`   | Fixed startup memory in GB.                               |
| `ProcessorCount`    | Number of virtual processors.                             |
| `VHDSizeGB`         | VHDX size in GB.                                          |
| `DisplayResolution` | Configured display resolution.                            |
| `Created`           | `$true` after successful VM creation.                     |
| `Started`           | `$true` when the function started the VM.                 |
| `Checkpointed`      | `$true` when the function created the initial checkpoint. |
| `StartVMConnect`    | `$true` when the function launched VMConnect.             |

A `-WhatIf` result omits `DisplayResolution` and sets all four action-status properties to `$false` because no VM actions occurred.

See [Create a Hyper-V VM](../basic/osdeploy-hypervm.md) for the basic workflow or the [New-OSDeployHyperVM command reference](../../command-reference/osdeploy/new-osdeployhypervm.md) for compact syntax and parameter definitions.

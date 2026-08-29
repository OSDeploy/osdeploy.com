---
description: Create a Hyper-V virtual machine for testing OSDeploy boot media.
---

# Create a Hyper-V VM

Use `New-OSDeployHyperVM` to create and start a Hyper-V virtual machine for testing OSDeploy boot media. The function creates the virtual disk, attaches the newest OSDeploy boot ISO, configures the VM, creates an initial checkpoint, and opens the Hyper-V console when VMConnect is available.

## Requirements

Run the function on a physical computer that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../module-setup/README.md)
* [Microsoft Hyper-V](../install-osdeploysoftware/microsoft-hyper-v.md) and the Hyper-V PowerShell tools
* Administrator rights
* `curl.exe` available in `PATH`
* Enough processor, memory, and storage capacity for the VM

{% hint style="warning" %}
Restart Windows after enabling Hyper-V. `New-OSDeployHyperVM` stops when Hyper-V is pending a restart, and it cannot run inside another virtual machine.
{% endhint %}

## Basic Usage

Open an elevated PowerShell 7.6 session and run the function without parameters:

```powershell
New-OSDeployHyperVM
```

The default command creates a timestamp-named Generation 2 VM similar to `260829-143000 OSDeploy` with this configuration:

| Setting | Default |
| --- | --- |
| Virtual processors | 2 |
| Startup memory | 8 GB, fixed |
| Virtual disk | 64 GB VHDX |
| Display resolution | `1600x900` |
| Secure Boot template | `MicrosoftWindows` |
| Initial checkpoint | Enabled |
| Start VM | Enabled |

{% hint style="info" %}
OSDeploy recommends at least 2 virtual processors and 12 GB of startup memory for smoother deployment work. Use 12 GB only when the physical host has enough memory for both Windows and the VM. The function default remains 8 GB.
{% endhint %}

## How It Works

`New-OSDeployHyperVM` performs the following actions:

1. Verifies Windows, PowerShell, administrator, physical-host, and Hyper-V requirements.
2. Searches `C:\ProgramData\OSDeployCore\boot` recursively for files named `bootmedia.iso` and selects the most recently modified file.
3. Uses `Default Switch` when available. If it is unavailable, the function uses the first Hyper-V switch returned by the host. When the host has no virtual switches, it creates the VM without a network connection.
4. Creates a timestamp-named VM and a VHDX in the Hyper-V host's default virtual hard disk location.
5. Adds a DVD drive and mounts the selected ISO. If no OSDeploy ISO is found, it adds an empty DVD drive.
6. Configures fixed memory, processors, display resolution, integration services, and VM lifecycle settings.
7. Configures the DVD drive as the first boot device and enables Secure Boot for a Generation 2 VM. When the host TPM is present and ready, the function also enables the available VM security and virtual TPM settings.
8. Creates the `New-OSDeployHyperVM` checkpoint.
9. Opens VMConnect when it is available, waits for the console to initialize, and starts the VM.

{% hint style="warning" %}
A VM created with an empty DVD drive does not have OSDeploy boot media to start. Attach an ISO before starting or restarting the VM.
{% endhint %}

For sizing, media, networking, firmware, checkpoint, and startup examples, see [New-OSDeployHyperVM](new-osdeployhypervm.md). For compact syntax and parameter definitions, see the [command reference](../../command-reference/osdeploy/new-osdeployhypervm.md).

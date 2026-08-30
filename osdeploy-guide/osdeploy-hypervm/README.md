---
description: Create a Hyper-V virtual machine for testing OSDeploy boot media.
---

# OSDeploy HyperVM

Use `New-OSDeployHyperVM` to create and start a Hyper-V virtual machine for testing OSDeploy boot media. The function creates the virtual disk, attaches the newest OSDeploy boot ISO, configures the VM, creates an initial checkpoint, and opens the Hyper-V console when VMConnect is available.

## Create the Virtual Machine

{% stepper %}
{% step %}
### Confirm the Requirements

Run the function on a physical computer that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../requirements/powershell-modules/)
* [Microsoft Hyper-V](../software/microsoft-hyper-v.md) and the Hyper-V PowerShell tools
* Administrator rights
* `curl.exe` available in `PATH`
* Enough processor, memory, and storage capacity for the VM

{% hint style="warning" %}
Restart Windows after enabling Hyper-V. `New-OSDeployHyperVM` stops when Hyper-V is pending a restart, and it cannot run inside another virtual machine.
{% endhint %}
{% endstep %}

{% step %}
### Create the VM

Open an elevated PowerShell 7.6 session and run the function without parameters:

```powershell
New-OSDeployHyperVM
```

The default command creates a timestamp-named Generation 2 VM similar to `260829-143000 OSDeploy` with this configuration:

| Setting              | Default            |
| -------------------- | ------------------ |
| Virtual processors   | 2                  |
| Startup memory       | 8 GB, fixed        |
| Virtual disk         | 64 GB VHDX         |
| Display resolution   | `1600x900`         |
| Secure Boot template | `MicrosoftWindows` |
| Initial checkpoint   | Enabled            |
| Start VM             | Enabled            |

{% hint style="info" %}
OSDeploy recommends at least 2 virtual processors and 12 GB of startup memory for smoother deployment work. Use 12 GB only when the physical host has enough memory for both Windows and the VM. The function default remains 8 GB.
{% endhint %}

The function selects the newest `bootmedia.iso` under `C:\ProgramData\OSDeployCore\boot`. It prefers `Default Switch`, then the first available Hyper-V switch, and finally creates the VM without networking when no switch exists. It creates the VHDX in the Hyper-V host's default location, mounts the ISO, configures firmware and available TPM security, creates the checkpoint, opens VMConnect, and starts the VM.

{% hint style="warning" %}
A VM created with an empty DVD drive does not have OSDeploy boot media to start. Attach an ISO before starting or restarting the VM.
{% endhint %}
{% endstep %}

{% step %}
### Verify the VM

Inspect the newest OSDeploy VM, its mounted media, and its initial checkpoint:

```powershell
$VM = Get-VM |
	Where-Object Name -Like '* OSDeploy' |
	Sort-Object -Property CreationTime -Descending |
	Select-Object -First 1

$VM | Select-Object Name, State, Generation, ProcessorCount, MemoryStartup
$VM | Get-VMDvdDrive | Select-Object Path
$VM | Get-VMSnapshot | Select-Object Name, CreationTime
```

Confirm that the VM is running and that the DVD drive contains the expected OSDeploy ISO. If the DVD path is empty, attach boot media before restarting the VM.
{% endstep %}
{% endstepper %}

For sizing, media, networking, firmware, checkpoint, and startup examples, see [New-OSDeployHyperVM](new-osdeployhypervm.md). For compact syntax and parameter definitions, see the [command reference](../../command-reference/osdeploy/new-osdeployhypervm.md).

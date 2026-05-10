# New-OSDeployHyperVM

Creates a Hyper-V virtual machine pre-configured for OSDeploy testing.

| Property | Value                                                                        |
|----------|------------------------------------------------------------------------------|
| Module   | OSDeploy                                                                     |
| Platform | Windows 11 (amd64 / arm64)                                                  |
| Requires | PowerShell 7.6, Hyper-V enabled, Run as Administrator                        |
| Output   | `System.Management.Automation.PSCustomObject`                                |

## Description

Creates a new Hyper-V VM with configurable generation, memory, processor count, virtual disk size, display resolution, virtual switch, and Secure Boot template. If `-ISO` is provided, the ISO is mounted to the VM's DVD drive. If `-ISO` is omitted, the function searches for the latest `bootmedia.iso` under `$env:ProgramData\OSDeployCore\boot-media\` and mounts it automatically. If no ISO is found, the VM is created with an empty DVD drive.

## Syntax

```powershell
New-OSDeployHyperVM [-ISO <String>] [-NamePrefix <String>] [-Generation <UInt16>]
    [-MemoryStartupGB <UInt16>] [-ProcessorCount <UInt16>] [-VHDSizeGB <UInt16>]
    [-DisplayResolution <String>] [-SwitchName <String>]
    [-SecureBootTemplate <String>] [-CheckpointVM <bool>] [-StartVM <bool>]
    [-WhatIf] [-Confirm]
```

## Parameters

| Parameter              | Type      | Required | Default                    | Description                                                                                                                                   |
|------------------------|-----------|----------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| `-ISO`                 | `String`  | No       | Auto-detected              | Path to a boot ISO to mount. Must be an `.iso` file. If omitted, the latest `bootmedia.iso` under OSDeployCore is used automatically.         |
| `-NamePrefix`          | `String`  | No       | `OSDeploy`                 | Prefix for the generated VM name. The final name is timestamped.                                                                              |
| `-Generation`          | `UInt16`  | No       | `2`                        | VM generation. Valid values: `1` or `2`.                                                                                                      |
| `-MemoryStartupGB`     | `UInt16`  | No       | `8`                        | Startup memory in GB. Valid range: 2–64.                                                                                                      |
| `-ProcessorCount`      | `UInt16`  | No       | `2`                        | Number of virtual processors. Valid range: 1–64.                                                                                              |
| `-VHDSizeGB`           | `UInt16`  | No       | `64`                       | Virtual disk size in GB. Valid range: 8–512.                                                                                                  |
| `-DisplayResolution`   | `String`  | No       | `1600x900`                 | Display resolution for the VM video adapter. Supports common resolutions from `640x480` to `4096x2160`.                                       |
| `-SwitchName`          | `String`  | No       | `Default Switch`           | Virtual switch name. Falls back to the first available switch if `Default Switch` is not found.                                               |
| `-SecureBootTemplate`  | `String`  | No       | `MicrosoftWindows`         | Secure Boot template for Generation 2 VMs. Valid values: `MicrosoftWindows`, `MicrosoftUEFICertificateAuthority`, `OpenSourceShieldedVM`.     |
| `-CheckpointVM`        | `bool`    | No       | `$true`                    | Create an initial checkpoint after VM creation.                                                                                               |
| `-StartVM`             | `bool`    | No       | `$true`                    | Start the VM after creation.                                                                                                                  |

## Examples

```powershell
# Create a VM with all defaults; auto-selects the latest bootmedia.iso
New-OSDeployHyperVM
```

```powershell
# Create a VM and mount a specific ISO; start the VM after creation
New-OSDeployHyperVM -ISO 'D:\ISO\WinPE.iso' -StartVM $true
```

```powershell
# Create a Generation 1 VM with 4 GB RAM and a 128 GB disk, do not start it
New-OSDeployHyperVM -Generation 1 -MemoryStartupGB 4 -VHDSizeGB 128 -StartVM $false
```

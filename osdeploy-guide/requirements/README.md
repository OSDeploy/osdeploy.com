# PC Requirements

The OSDeploy PC is the Windows 11 workstation where you prepare OSDeploy Core and create boot media, while OSDCloud runs from that WinPE media on the target device, and performs the Windows 11 deployment.

{% hint style="info" %}
Use a dedicated physical PC or virtual machine to keep the OSDeploy tools, source content, configuration, and build output isolated and reproducible.
{% endhint %}

## Baseline

Configure the OSDeploy PC with the following baseline:

| Requirement        | Baseline                                                                                                         |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| Environment        | Dedicated physical PC or virtual machine                                                                         |
| Operating system   | Windows 11 25H2 amd64                                                                                            |
| Permissions        | Local administrative rights                                                                                      |
| Storage            | At least 50 GB of free space on the system volume                                                                |
| Network            | Internet access to Microsoft, GitHub, PowerShell Gallery, Recast Software, and hardware-vendor download services |
| PowerShell         | Latest PowerShell 7 installed from the MSI package                                                               |
| PowerShell modules | OSDeploy and OSDCloud; OSD is optional                                                                           |

Windows 11 on arm64 can potentially be used, but this configuration is not fully tested. Other Windows client versions and Windows Server are unsupported.

## Setup

Complete the OSDeploy PC setup in this order:

1. [Configure Windows 11](windows-11-os.md) and install current updates.
2. Install the latest [PowerShell 7 MSI package](powershell-7.md).
3. Install the required [PowerShell modules](powershell-modules.md).
4. Optionally complete [Community Registration](../registration/).

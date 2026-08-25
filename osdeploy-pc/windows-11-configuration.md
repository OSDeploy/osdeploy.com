# Windows 11 Configuration

Configure the Windows optional features required by the OSDeploy workflows that will run on the OSDeploy PC. These changes require local administrative rights and may require a restart.

{% embed url="https://learn.microsoft.com/en-us/windows/client-management/client-tools/add-remove-hide-features" %}

## Hyper-V

Hyper-V provides the local virtualization platform used by `New-OSDeployHyperVM` to create test virtual machines that boot from OSDeploy WinPE images. Use it to validate boot images and test OSDCloud deployments without physical hardware.

Hyper-V is available in Windows 11 Pro, Enterprise, and Education. It is optional for boot-image creation and OSDCloud deployment, and is required only when the OSDeploy PC will run local test virtual machines.

{% hint style="warning" %}
Enabling Hyper-V requires a physical OSDeploy PC with hardware virtualization enabled. Restart Windows after enabling the feature before creating or starting virtual machines.
{% endhint %}

### Enable Hyper-V with OSDeploy

Run the following command from an elevated PowerShell 7 session to preview the change:

```powershell
Install-OSDeploySoftware -Name 'hyperv'
```

Enable Hyper-V:

```powershell
Install-OSDeploySoftware -Name 'hyperv' -Force
```

OSDeploy skips Hyper-V when it detects that the OSDeploy PC is a virtual machine. The component does not add files to OSDeploy Core and does not support `-DownloadOnly`.

### Enable Hyper-V Manually

Run the following command from an elevated PowerShell session:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All' -All -NoRestart
```

Alternatively, enable Hyper-V from Windows Features:

1. Open **Control Panel** > **Programs** > **Turn Windows features on or off**.
2. Expand **Hyper-V** and select all subfeatures.
3. Select **OK**, then restart Windows.

### Verify Hyper-V

After restarting Windows, confirm that the feature is enabled:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All'
```

Confirm that `State` is `Enabled`.

## Related

* [Microsoft Hyper-V](install-software/microsoft-hyper-v.md)
* [New-OSDeployHyperVM](../powershell-modules/osdeploy/New-OSDeployHyperVM.md)

Continue to [Install PowerShell 7](install-powershell-7.md).

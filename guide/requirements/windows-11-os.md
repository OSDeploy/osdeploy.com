---
description: Prepare a supported Windows 11 workstation for OSDeploy.
---

# Windows 11

Use a fully updated Windows 11 25H2 amd64 computer as the OSDeploy PC.

## Prepare the OSDeploy PC

{% stepper %}
{% step %}
### Confirm the Requirements

Use a workstation that meets these requirements:

* Windows 11 25H2, build 26200 or later
* amd64 architecture
* Local administrative rights
* At least 50 GB of free space on the system volume
* Internet access to Microsoft, GitHub, PowerShell Gallery, Recast Software, and required hardware-vendor download services

Proxy servers and TLS inspection must permit PowerShell, `curl.exe`, and OSDeploy to download content from these services.

{% hint style="warning" %}
Windows 11 on arm64 can potentially be used, but it is not fully tested. Other Windows client versions and all Windows Server versions are unsupported.
{% endhint %}
{% endstep %}

{% step %}
### Check the OSDeploy PC

Open PowerShell as an administrator and run:

```powershell
$OperatingSystem = Get-CimInstance -ClassName Win32_OperatingSystem
$SystemDrive = Get-CimInstance -ClassName Win32_LogicalDisk -Filter "DeviceID='$env:SystemDrive'"
$IsAdministrator = ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole(
	[Security.Principal.WindowsBuiltInRole]::Administrator
)

[pscustomobject]@{
	ProductName    = $OperatingSystem.Caption
	DisplayVersion = (Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion').DisplayVersion
	BuildNumber    = [int]$OperatingSystem.BuildNumber
	Architecture   = $env:PROCESSOR_ARCHITECTURE
	IsAdministrator = $IsAdministrator
	FreeSpaceGB    = [math]::Round($SystemDrive.FreeSpace / 1GB, 1)
}
```

Confirm that the output reports Windows 11 25H2, build 26200 or later, `AMD64`, administrative rights, and at least 50 GB of free space. `ARM64` is possible but not fully tested.
{% endstep %}

{% step %}
### Update Windows

Open Windows Update:

```powershell
Start-Process 'ms-settings:windowsupdate'
```

Select **Check for updates**, install all available cumulative updates, and restart Windows when prompted. Repeat the process until Windows Update reports that the OSDeploy PC is current.
{% endstep %}

{% step %}
### Enable Hyper-V

Enable Hyper-V now so its required restart is complete before building and testing OSDeploy boot media. Run this command on a physical Windows 11 Pro, Enterprise, or Education PC with hardware virtualization enabled:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V-All' -All -NoRestart
```

Restart Windows after the command completes. See [Microsoft Hyper-V](../cmdlets/install-osdeploysoftware/microsoft-hyper-v.md) for requirements and additional installation options.
{% endstep %}
{% endstepper %}

Continue to [Install PowerShell 7](powershell-7.md).

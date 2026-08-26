# Windows 11 OS

Use a fully updated Windows 11 25H2 amd64 computer as the OSDeploy PC.

## Supported Configuration

| Component        | Requirement                                                |
| ---------------- | ---------------------------------------------------------- |
| Operating system | Windows 11 25H2, build 26200 or later                      |
| Architecture     | amd64                                                      |
| PowerShell       | Latest stable PowerShell 7, installed from the MSI package |
| Permissions      | Local administrative rights                                |
| Storage          | At least 50 GB of free space on the system volume          |
| Network          | Internet access                                            |

{% hint style="warning" %}
Windows 11 on arm64 can potentially be used, but it is not fully tested. Other Windows client versions and all Windows Server versions are unsupported.
{% endhint %}

The OSDeploy module manages the Windows ADK components required by its boot-image workflow. Do not install the ADK as part of this OSDeploy PC setup.

## Check the OSDeploy PC

Open Windows PowerShell or PowerShell as an administrator, then run the following commands:

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

## Update Windows

The OSDeploy PC must have the latest cumulative updates before OSDeploy creates or services boot images.

Open Windows Update from PowerShell:

```powershell
Start-Process 'ms-settings:windowsupdate'
```

Select **Check for updates**, install all available cumulative updates, and restart Windows when prompted. Repeat the process until Windows Update reports that the OSDeploy PC is current.

## Internet Access

Allow outbound HTTPS access to the services used by the OSDeploy PC. These include:

* Microsoft download and update services
* GitHub release and content services
* PowerShell Gallery
* Recast Software Community Portal
* Dell, HP, Lenovo, Intel, Microsoft Surface, and other hardware-vendor download services used for drivers

Proxy servers and TLS inspection must permit PowerShell, `curl.exe`, and OSDeploy to download content from these services.

Continue to [Windows 11 Configuration](windows-11-optional.md).

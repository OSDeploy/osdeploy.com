# System Requirements

Use a fully updated Windows 11 25H2 amd64 computer for the OSDeploy Core Workstation.

## Supported Configuration

| Component | Requirement |
| --- | --- |
| Operating system | Windows 11 25H2, build 26200 or later |
| Architecture | amd64 |
| PowerShell | Latest stable PowerShell 7, installed from the MSI package |
| Permissions | Local administrative rights |
| Storage | At least 50 GB of free space on the system volume |
| Network | Internet access |

{% hint style="warning" %}
Windows 11 on arm64 can potentially be used, but it is not fully tested. Other Windows client versions and all Windows Server versions are unsupported.
{% endhint %}

The OSDeploy module manages the Windows ADK components required by its boot-image workflow. Do not install the ADK as part of this workstation setup.

## Check the Workstation

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

The workstation must have the latest cumulative updates before OSDeploy creates or services boot images.

Open Windows Update from PowerShell:

```powershell
Start-Process 'ms-settings:windowsupdate'
```

Select **Check for updates**, install all available cumulative updates, and restart Windows when prompted. Repeat the process until Windows Update reports that the workstation is current.

## Internet Access

Allow outbound HTTPS access to the services used by the workstation. These include:

* Microsoft download and update services
* GitHub release and content services
* PowerShell Gallery
* Recast Software Community Portal
* Dell, HP, Lenovo, Intel, Microsoft Surface, and other hardware-vendor download services used for drivers

Proxy servers and TLS inspection must permit PowerShell, `curl.exe`, and OSDeploy to download content from these services.

## Install or Update PowerShell 7

Run the following steps from an elevated Windows PowerShell 5.1 session to install PowerShell 7 for the first time. Use the same steps from an elevated PowerShell 7 session to update an earlier PowerShell 7 release.

PowerShell 7 installs side-by-side with Windows PowerShell 5.1. It does not replace Windows PowerShell.

{% hint style="warning" %}
Install PowerShell 7 from the MSI package. Beginning with PowerShell 7.6, WinGet installs the MSIX package by default. The MSIX sandbox prevents system-level operations required by DISM and OSDeploy.
{% endhint %}

### Download the Latest MSI

This script reads the latest stable PowerShell release from GitHub, detects the workstation architecture, and downloads the matching MSI package to the OSDeploy Core software cache.

```powershell
$Release = Invoke-RestMethod -Uri 'https://api.github.com/repos/PowerShell/PowerShell/releases/latest'
$Version = $Release.tag_name.TrimStart('v')
$Architecture = if ($env:PROCESSOR_ARCHITECTURE -eq 'ARM64') { 'arm64' } else { 'x64' }
$MsiFileName = "PowerShell-$Version-win-$Architecture.msi"
$DownloadFolder = Join-Path $env:ProgramData 'OSDeployCore\software\Microsoft.PowerShell'
$MsiPath = Join-Path $DownloadFolder $MsiFileName

New-Item -Path $DownloadFolder -ItemType Directory -Force | Out-Null

$Asset = $Release.assets | Where-Object name -EQ $MsiFileName
if (-not $Asset) {
	throw "The $MsiFileName asset was not found in the latest stable PowerShell release."
}

curl.exe -L -o $MsiPath $Asset.browser_download_url
```

### Install the MSI

Install or update PowerShell 7 with the recommended system integration and Microsoft Update options:

```powershell
$MsiParameters = @(
	"/package `"$MsiPath`""
	'/quiet'
	'/norestart'
	'ADD_EXPLORER_CONTEXT_MENU_OPENPOWERSHELL=1'
	'ADD_FILE_CONTEXT_MENU_RUNPOWERSHELL=1'
	'ENABLE_PSREMOTING=1'
	'REGISTER_MANIFEST=1'
	'USE_MU=1'
	'ENABLE_MU=1'
	'ADD_PATH=1'
)

& msiexec.exe $MsiParameters
if ($LASTEXITCODE -notin 0, 3010) {
	throw "PowerShell MSI installation failed with exit code $LASTEXITCODE."
}
```

Exit the current shell and open a new PowerShell 7 session by running `pwsh`. Verify the installation:

```powershell
$PSVersionTable | Select-Object PSVersion, PSEdition, OS
[System.Runtime.InteropServices.RuntimeInformation]::OSArchitecture
```

Confirm that `PSEdition` is `Core` and that the reported version is the stable release downloaded above. Restart Windows if `msiexec.exe` returned exit code `3010`.

See [Install PowerShell 7](../core-components/powershell/install-powershell.md) for additional MSI package details.

Continue to [PowerShell Modules](powershell-modules.md).

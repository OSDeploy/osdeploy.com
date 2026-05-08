# Install Windows ADK 25H2

Installs the Windows Assessment and Deployment Kit version **10.1.26100.2454** (Windows 11 25H2) and its required WinPE add-on.

| Property | Value |
|---|---|
| Version | 10.1.26100.2454 |
| Architecture | amd64 / arm64 |
| Platform | Windows 11 |
| ADK setup | `https://go.microsoft.com/fwlink/?linkid=2289980` |
| WinPE add-on setup | `https://go.microsoft.com/fwlink/?linkid=2289981` |
| Cache path | `C:\ProgramData\OSDeployCore\Software\Microsoft.WindowsADK_10.1.26100.2454\` |

{% embed url="https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install" %}
{% endembed %}

{% hint style="info" %}
The ADK and the WinPE add-on are two separate downloads and must be the same version. Both are installed when using `Install-OSDeploySoftware`. The MDT WinPE x86 MMC snap-in bugfix is applied automatically during installation.
{% endhint %}

---

## Install with OSDeploy

The OSDeploy module downloads both installers and runs them silently in sequence. Administrator rights are required.

**Preview (no changes made):**

```powershell
Install-OSDeploySoftware -Name 'adk-25h2'
```

**Install:**

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -Force
```

**Download only (skip installation):**

```powershell
Install-OSDeploySoftware -Name 'adk-25h2' -DownloadOnly -Force
```

Installers are cached to `C:\ProgramData\OSDeployCore\Software\Microsoft.WindowsADK_10.1.26100.2454\` and reused on subsequent runs.

{% hint style="info" %}
Requires PowerShell 7.6 or later running as Administrator. `curl.exe` must be available in `PATH` (included with Windows 10 1803 and later). `winget` is not used for ADK installation.
{% endhint %}

---

## Manual Installation

To install the ADK manually, download both setup executables and run them silently.

**Download ADK setup and WinPE add-on setup:**

```powershell
$DownloadPath = "$env:ProgramData\OSDeployCore\Software\Microsoft.WindowsADK_10.1.26100.2454"
New-Item -Path $DownloadPath -ItemType Directory -Force | Out-Null

curl.exe --location --output "$DownloadPath\adksetup.exe" --url 'https://go.microsoft.com/fwlink/?linkid=2289980'
curl.exe --location --output "$DownloadPath\adkwinpesetup.exe" --url 'https://go.microsoft.com/fwlink/?linkid=2289981'
```

**Install Windows ADK:**

```powershell
Start-Process -FilePath "$DownloadPath\adksetup.exe" -ArgumentList '/quiet', '/norestart' -Wait
```

**Install WinPE add-on:**

```powershell
Start-Process -FilePath "$DownloadPath\adkwinpesetup.exe" -ArgumentList '/quiet', '/norestart' -Wait
```

**Apply MDT WinPE x86 bugfix (required for MDT integration):**

```powershell
$x86Path = 'C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs'
if (-not (Test-Path $x86Path)) {
    New-Item -Path $x86Path -ItemType Directory -Force | Out-Null
}
```

{% hint style="info" %}
Administrator rights are required for both setup steps.
{% endhint %}

---

## Related

- [Windows ADK documentation](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)
- [What's new in kits and tools](https://learn.microsoft.com/en-us/windows-hardware/get-started/what-s-new-in-kits-and-tools)
- [Install-OSDeploySoftware](https://docs.osdeploy.com)
- [Install Windows ADK 26H1](install-26h1.md)

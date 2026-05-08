# Install MDT

{% hint style="warning" %}
Microsoft Deployment Toolkit has been officially retired by Microsoft. Install it only if you have an existing MDT-based workflow that requires it.

[MDT Retirement Notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement)
{% endhint %}

MDT version **6.3.8456.1000** is the final release. The installer is no longer hosted on Microsoft's servers and must be downloaded from the Internet Archive.

| Property | Value |
|---|---|
| Version | 6.3.8456.1000 |
| Architecture | x64 |
| SHA256 | `dabfd183c525bdb4866d2d9324f064a291ca62f3a16ac429cf3338be529d1d58` |

---

## Install MDT with OSDeploy

The OSDeploy module handles the download, SHA256 verification, and silent installation in a single command. Administrator rights are required.

**Preview (no changes made):**

```powershell
Install-OSDeploySoftware -Name 'mdt'
```

**Install:**

```powershell
Install-OSDeploySoftware -Name 'mdt' -Force
```

**Download only (skip installation):**

```powershell
Install-OSDeploySoftware -Name 'mdt' -DownloadOnly -Force
```

The MSI is cached to `C:\ProgramData\OSDeployCore\cache\downloads\Microsoft.DeploymentToolkit_6.3.8456.1000\` and the SHA256 checksum is verified before installation proceeds.

{% hint style="info" %}
Requires PowerShell 7.6 or later running as Administrator. `curl.exe` must be available in `PATH` (included with Windows 10 1803 and later).
{% endhint %}

---

## Install MDT

To install MDT manually, download the MSI from the Internet Archive, verify the SHA256 checksum, and run a silent install with `msiexec`.

**Download:**

```powershell
$mdtUrl = 'https://web.archive.org/web/20250616094712/https://download.microsoft.com/download/3/3/9/339BE62D-B4B8-4956-B58D-73C4685FC492/MicrosoftDeploymentToolkit_x64.msi'
$installerPath = "$env:TEMP\MicrosoftDeploymentToolkit_x64.msi"

curl.exe --location --output $installerPath --url $mdtUrl
```

**Verify SHA256:**

```powershell
$expectedSha256 = 'dabfd183c525bdb4866d2d9324f064a291ca62f3a16ac429cf3338be529d1d58'
$actualSha256 = (Get-FileHash -Path $installerPath -Algorithm SHA256).Hash.ToLowerInvariant()

if ($actualSha256 -ne $expectedSha256) {
    throw "Checksum mismatch. Expected $expectedSha256 but got $actualSha256."
}
```

**Install silently:**

```powershell
Start-Process -FilePath 'msiexec.exe' -ArgumentList '/i', $installerPath, '/qn', '/norestart' -Wait
```

{% hint style="info" %}
Administrator rights are required for the `msiexec` install step.
{% endhint %}

---

## Related

- [Microsoft Deployment Toolkit documentation](https://learn.microsoft.com/en-us/intune/configmgr/mdt/)
- [MDT Retirement Notice](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/mdt/mdt-retirement)
- [Install-OSDeploySoftware](https://docs.osdeploy.com)

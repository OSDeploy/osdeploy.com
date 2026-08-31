---
description: Install PowerShell 7.6 or later from the MSI package required by OSDeploy.
---

# Install PowerShell 7

Install the latest stable PowerShell 7 MSI package on the OSDeploy PC. PowerShell 7 installs side by side with Windows PowerShell 5.1 and runs from `$env:ProgramFiles\PowerShell\7` by using `pwsh`.

{% hint style="warning" %}
Do not use the default WinGet command to install PowerShell for OSDeploy. Beginning with PowerShell 7.6.0, WinGet installs the MSIX package by default. That package runs from `WindowsApps`, and OSDeploy rejects it because the app-container installation does not support the required PowerShell DISM operations. Use the MSI workflow below.
{% endhint %}

## Install PowerShell

{% stepper %}
{% step %}
### Confirm the Requirements

Run these steps on a prepared [Windows 11 OSDeploy PC](windows-11-os.md) with Internet access. Open Windows PowerShell as an administrator.
{% endstep %}

{% step %}
### Download the Latest MSI

Run this script to detect the OSDeploy PC architecture and download the matching MSI package from the latest stable PowerShell release:

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
{% endstep %}

{% step %}
### Install the MSI

Silently install PowerShell with the recommended options from Microsoft:

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

See [Install the MSI package with command-line options](https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6#install-the-msi-package-with-command-line-options) for details about these installer properties.
{% endstep %}

{% step %}
### Open PowerShell 7

Exit Windows PowerShell, open a new PowerShell 7 session by running `pwsh`, and inspect the installed version and location:

```powershell
$PSVersionTable | Select-Object PSEdition, PSVersion
$PSHOME
```

The output must report `Core`, PowerShell 7.6 or later, and a `$PSHOME` under `$env:ProgramFiles\PowerShell\7`. Restart Windows first if `msiexec.exe` returned exit code `3010`.
{% endstep %}
{% endstepper %}

Continue to [Install PowerShell Modules](powershell-modules.md).

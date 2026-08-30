# PowerShell 7

{% embed url="https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6" %}

OSDeploy requires **PowerShell 7.6 or later**. PowerShell 7 installs side-by-side with Windows PowerShell 5.1 and does not replace it. It is installed to `$env:ProgramFiles\PowerShell\7` and launched using `pwsh`.

These instructions target **Windows only** (amd64 and arm64). The snippets detect the current architecture automatically.

{% hint style="info" %}
PowerShell releases frequently. Always check the [PowerShell GitHub Releases](https://github.com/PowerShell/PowerShell/releases/latest) page for the current version and update the `$Version` variable accordingly.
{% endhint %}

## Why Not WinGet?

Beginning with PowerShell 7.6.0, WinGet installs the **MSIX package by default**. MSIX packages run inside an application sandbox that virtualizes filesystem and registry access. This sandbox **blocks system-level operations**, including commands that call `dism.exe`. This makes MSIX-based installations incompatible with OSD workflows such as OSDCloud.

{% hint style="warning" %}
Use the **MSI package** for all system and enterprise deployment scenarios.
{% endhint %}

## Download the Latest MSI

This script reads the latest stable PowerShell release from GitHub, detects the OSDeploy PC architecture, and downloads the matching MSI package to the OSDeploy Core software cache.

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

## Install the MSI

The following snippet silently installs the MSI with all recommended options enabled. Source: [Install the MSI package with command-line options](https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6#install-the-msi-package-with-command-line-options).

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

Continue to [Install PowerShell Modules](powershell-modules/).

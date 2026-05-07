# PowerShell

{% embed url="https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6" %}

OSDeploy requires **PowerShell 7.6 or later**. PowerShell 7 installs side-by-side with Windows PowerShell 5.1 and does not replace it. It is installed to `$env:ProgramFiles\PowerShell\7` and launched using `pwsh`.

These instructions target **Windows only** (amd64 and arm64). The snippets detect the current architecture automatically.

> **Note:** PowerShell releases frequently. Always check the [PowerShell GitHub Releases](https://github.com/PowerShell/PowerShell/releases/latest) page for the current version and update the `$Version` variable accordingly.

## Why Not WinGet?

Beginning with PowerShell 7.6.0, WinGet installs the **MSIX package by default**. MSIX packages run inside an application sandbox that virtualizes filesystem and registry access. This sandbox **blocks system-level operations**, including commands that call `dism.exe`. This makes MSIX-based installations incompatible with OSD workflows such as OSDCloud.

> Use the **MSI package** for all system and enterprise deployment scenarios.

## Download the MSI

The following snippet detects the system architecture, creates the download directory, and saves the installer to `$env:ProgramData\OSDeployCore\Software\Microsoft.PowerShell`.

`curl.exe` is used for downloads — it is faster and more reliable than `Invoke-WebRequest` in Windows system contexts.

```powershell
# Update $Version to match the latest release:
# https://github.com/PowerShell/PowerShell/releases/latest
$Version        = '7.6.1'
$Arch           = if ($env:PROCESSOR_ARCHITECTURE -eq 'ARM64') { 'arm64' } else { 'x64' }
$MsiFileName    = "PowerShell-$Version-win-$Arch.msi"
$MsiUrl         = "https://github.com/PowerShell/PowerShell/releases/download/v$Version/$MsiFileName"
$DownloadFolder = "$env:ProgramData\OSDeployCore\Software\Microsoft.PowerShell"
$MsiPath        = Join-Path $DownloadFolder $MsiFileName

if (-not (Test-Path $DownloadFolder)) {
    New-Item -Path $DownloadFolder -ItemType Directory -Force | Out-Null
}

curl.exe -L -o $MsiPath $MsiUrl
```

## Install the MSI

The following snippet silently installs the MSI with all recommended options enabled. Source: [Install the MSI package with command-line options](https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6#install-the-msi-package-with-command-line-options).

```powershell
# Update $Version to match the version you downloaded
$Version  = '7.6.1'
$Arch     = if ($env:PROCESSOR_ARCHITECTURE -eq 'ARM64') { 'arm64' } else { 'x64' }
$MsiPath  = "$env:ProgramData\OSDeployCore\Software\Microsoft.PowerShell\PowerShell-$Version-win-$Arch.msi"

$msiParams = @(
    "/package `"$MsiPath`""
    '/quiet'
    'ADD_EXPLORER_CONTEXT_MENU_OPENPOWERSHELL=1'
    'ADD_FILE_CONTEXT_MENU_RUNPOWERSHELL=1'
    'ENABLE_PSREMOTING=1'
    'REGISTER_MANIFEST=1'
    'USE_MU=1'
    'ENABLE_MU=1'
    'ADD_PATH=1'
)
msiexec.exe @msiParams
```

## After Installation

PowerShell 7 is installed to `$env:ProgramFiles\PowerShell\7`. Launch it from the Start Menu or by running `pwsh` in any terminal. The installer adds `pwsh` to the system `PATH` automatically when `ADD_PATH=1` is set.

To verify the installation:

```powershell
pwsh --version
```

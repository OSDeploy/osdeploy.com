---
description: Install PowerShell 7.6 or later from the MSI package required by OSDeploy.
---

# PowerShell 7

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

Follow Microsoft's guidance on installing the latest PowerShell 7 MSI:

{% embed url="https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell-on-windows?view=powershell-7.6#install-the-msi-package" %}
{% endstep %}

{% step %}
### MSI Parameters

Install PowerShell with all available options (recommended):

```powershell
$msiParams = @(
    '/package'
    'PowerShell-7.6.6-win-x64.msi'
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

{% step %}
### Windows Terminal

Make sure to set and save PowerShell as the Default profile in Windows Terminal, replacing Windows PowerShell.

<figure><img src="../../.gitbook/assets/image (695).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

Continue to [Install PowerShell Modules](powershell-modules.md).

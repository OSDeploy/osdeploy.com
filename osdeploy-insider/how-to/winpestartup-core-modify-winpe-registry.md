---
description: Apply WinPE registry settings from a .reg file in the shared WinpeStartup Core main folder.
---

# WinpeStartup Core: Modify the WinPE Registry with a .reg File

Add a `.reg` file to the shared WinpeStartup Core `main` folder to apply registry settings when WinPE starts.

{% hint style="warning" %}
Registry files modify the running WinPE registry. Review the keys and values before adding a `.reg` file to a build.
{% endhint %}

## Apply Registry Settings

{% stepper %}
{% step %}
### Create the main Folder

Create the shared folder on the OSDeploy build computer:

```powershell
$MainDirectory = Join-Path $env:ProgramData 'OSDeployCore\boot-assets\winpestartup-core\main'
New-Item -Path $MainDirectory -ItemType Directory -Force | Out-Null
```
{% endstep %}

{% step %}
### Add a .reg File

Create a standard Windows Registry Editor `.reg` file. This example adds a value under `HKLM\SOFTWARE\Contoso\WinPE`:

```powershell
@'
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Contoso\WinPE]
"BuildTag"="BranchOffice"
'@ | Set-Content -LiteralPath (Join-Path $MainDirectory 'Contoso.reg') -Encoding utf8
```

Replace the example key and value with the settings required by your WinPE workflow.
{% endstep %}

{% step %}
### Build WinPE Media

Run the build command. OSDeploy includes the shared `main` folder automatically when it is present:

```powershell
Build-OSDeployBoot
```

Boot the resulting media to apply the registry settings.
{% endstep %}
{% endstepper %}

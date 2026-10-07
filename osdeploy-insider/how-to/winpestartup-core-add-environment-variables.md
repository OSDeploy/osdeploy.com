---
description: Set WinPE environment variables from a .env file in the shared WinpeStartup Core main folder.
---

# WinpeStartup Core: Add WinPE Environment Variables with a .env File

Add a `.env` file to the shared WinpeStartup Core `main` folder to set environment variables when WinPE starts.

## Add the Environment Variables

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
### Create the .env File

Write one `NAME=value` entry per line. For example:

```powershell
@'
CONTOSO_SITE=BranchOffice
'@ | Set-Content -LiteralPath (Join-Path $MainDirectory '.env') -Encoding utf8
```

Use your own variable names and values. In WinPE, read a variable in PowerShell with `$env:CONTOSO_SITE`.
{% endstep %}

{% step %}
### Build WinPE Media

Run the build command. OSDeploy includes the shared `main` folder automatically when it is present:

```powershell
Build-OSDeployBoot
```

Boot the resulting media to use the configured variables.
{% endstep %}
{% endstepper %}

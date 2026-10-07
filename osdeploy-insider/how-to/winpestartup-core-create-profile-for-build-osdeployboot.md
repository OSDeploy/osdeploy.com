---
description: Create an optional WinpeStartup Core profile folder that can be selected when building WinPE media.
---

# WinpeStartup Core: Create a New Core Profile for Build-OSDeployBoot

Create a named folder under `winpestartup-core` when a customization should be optional. `Build-OSDeployBoot` displays every folder not named `main` in the WinpeStartup Core selection window.

## Create and Select the Core Profile

{% stepper %}
{% step %}
### Create the Profile Folder

Choose a descriptive folder name. This example creates a profile named `Contoso`:

```powershell
$CoreProfile = Join-Path $env:ProgramData `
    'OSDeployCore\boot-assets\winpestartup-core\Contoso'

New-Item -Path $CoreProfile -ItemType Directory -Force | Out-Null
```

Do not name the folder `main`. The `main` folder is included automatically and does not appear in the selection window.
{% endstep %}

{% step %}
### Add the Customization Files

Add the files required by this profile. For example, create an environment variable:

```powershell
@'
CONTOSO_SITE=BranchOffice
'@ | Set-Content -LiteralPath (Join-Path $CoreProfile '.env') -Encoding utf8
```

The folder can also contain `.reg` registry files and trusted root certificates in `.cer` format.
{% endstep %}

{% step %}
### Select the Profile During the Build

Start an interactive build:

```powershell
Build-OSDeployBoot
```

In the **Select WinpeStartup Core folders to add to this boot image** window, select `Contoso`, then complete the remaining build prompts. You can select more than one Core profile or cancel to skip all optional profiles.

OSDeploy copies the selected folder to `WinpeStartup\core\Contoso` in the mounted image. Boot the resulting media to run the selected customizations.
{% endstep %}
{% endstepper %}

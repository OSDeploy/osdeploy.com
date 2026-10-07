---
description: Automatically include shared WinpeStartup Core customizations in each OSDeploy WinPE build.
---

# WinpeStartup Core: Automate Customizations Using the main Folder

Store shared WinPE startup customizations in the `main` folder. `Build-OSDeployBoot` includes this folder automatically when it is present, without requiring a selection in the optional WinpeStartup Core folder picker.

## Add Shared Customizations

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
### Add Your Customization Files

Copy the files WinPE should use into `main`. For example, add a `.env` file, a `.reg` file, or a root certificate in `.cer` format.

See [Add WinPE Environment Variables](./winpestartup-core-add-environment-variables.md), [Modify the WinPE Registry](./winpestartup-core-modify-winpe-registry.md), and [Import a WinPE Root Certificate](./winpestartup-core-import-root-certificate.md) for examples.
{% endstep %}

{% step %}
### Build WinPE Media

Run the build command:

```powershell
Build-OSDeployBoot
```

OSDeploy copies the shared `main` folder into `WinpeStartup\core\main` in the mounted image. Boot the resulting media to run the customizations.
{% endstep %}
{% endstepper %}

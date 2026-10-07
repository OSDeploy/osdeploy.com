---
description: Import a WinPE root certificate from a .cer file in the shared WinpeStartup Core main folder.
---

# WinpeStartup Core: Import a WinPE Root Certificate with a .cer File

Place a `.cer` root certificate in the shared WinpeStartup Core `main` folder to import it when WinPE starts.

{% hint style="warning" %}
Only add a root certificate after verifying that it belongs to a certificate authority you intend to trust.
{% endhint %}

## Import the Certificate

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
### Copy the .cer File

Copy your verified root certificate into the folder. Replace the source path with the location of your certificate:

```powershell
Copy-Item -LiteralPath 'C:\Certificates\ContosoRoot.cer' -Destination $MainDirectory
```
{% endstep %}

{% step %}
### Build WinPE Media

Run the build command. OSDeploy includes the shared `main` folder automatically when it is present:

```powershell
Build-OSDeployBoot
```

Boot the resulting media to import the certificate into WinPE's local-machine trusted root certificate store.
{% endstep %}
{% endstepper %}

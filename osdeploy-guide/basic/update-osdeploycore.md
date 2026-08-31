---
description: >-
  Download and prepare the Windows, recovery, and driver content used by
  OSDeploy.
---

# Update Core Content

Use `Update-OSDeployCore` to prepare the local content library used to build OSDeploy boot images. The command downloads current Windows source files, prepares Windows and Windows Recovery Environment (WinRE) content, and updates Windows Preinstallation Environment (WinPE) drivers under `$env:ProgramData\OSDeployCore`.

## Prepare OSDeployCore

{% stepper %}
{% step %}
### Confirm the Requirements

Run the function on a workstation that meets these requirements:

* Windows 11 25H2 build 26200 or later
* PowerShell 7.6 or later installed from the MSI package
* Current [OSDeploy module](../requirements/powershell-modules.md)
* [Required software](install-osdeploysoftware.md)
* Administrator rights
* `curl.exe` available in `PATH`
* Internet access and enough storage for Windows source files and drivers

{% hint style="warning" %}
Windows source files are large, and preparing the images can take time. Keep the elevated PowerShell session open until the command finishes.
{% endhint %}
{% endstep %}

{% step %}
### Run the Update

Open an elevated PowerShell 7.6 session and run:

```powershell
Update-OSDeployCore
```

Follow the confirmation prompts to download the Windows source files, prepare the Windows and WinRE content, and update the WinPE drivers. Existing verified content can be reused.
{% endstep %}

{% step %}
### Verify OSDeployCore

Inspect the prepared Windows, WinRE, and driver content:

```powershell
Get-ChildItem -Path "$env:ProgramData\OSDeployCore\cache\windows-os" -Directory
Get-ChildItem -Path "$env:ProgramData\OSDeployCore\cache\windows-re" -Directory
Get-ChildItem -Path "$env:ProgramData\OSDeployCore\repository\build-winpedrivers" -Directory
```

Confirm that the commands return prepared content, then continue to [Build an OSDeploy Boot Image](build-boot.md).
{% endstep %}
{% endstepper %}

For update behavior and advanced examples, see [Update-OSDeployCore](../advanced/osdeploycore/update-osdeploycore.md). For compact syntax, see the [Update-OSDeployCore command reference](../../command-reference/osdeploy/update-osdeploycore.md).

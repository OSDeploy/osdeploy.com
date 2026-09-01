---
description: Install 7-Zip and prepare its WinPE files with Install-OSDeploySoftware.
---

# 7-Zip

7-Zip provides archive support on the OSDeploy PC and in OSDeploy boot images. `Install-OSDeploySoftware` installs the `7zip.7zip` WinGet package and prepares the files used to add 7-Zip to WinPE.

{% hint style="info" %}
7-Zip is required for the standard OSDeploy and OSDCloud workflow.
{% endhint %}

## Preview

Review the package source and install command without making changes:

```powershell
Install-OSDeploySoftware -Name '7zip'
```

## Install

Run the installation from an elevated PowerShell 7.6 or later session:

```powershell
Install-OSDeploySoftware -Name '7zip' -Force
```

The function performs three operations:

1. Downloads amd64 and arm64 installers to `C:\ProgramData\OSDeployCore\software\7zip.7zip`.
2. Installs 7-Zip on the OSDeploy PC when `C:\Program Files\7-Zip\7z.exe` is not present.
3. Downloads and extracts the versioned 7-Zip files used by OSDeploy boot images to `C:\ProgramData\OSDeployCore\cache\winpe-apps\7zip`.

Existing architecture-specific installer folders and the current WinPE cache are reused. Older WinPE cache versions are removed.

## Download Only

Prepare both architecture-specific installers and the WinPE app cache without installing 7-Zip on the OSDeploy PC:

```powershell
Install-OSDeploySoftware -Name '7zip' -DownloadOnly
```

## Related

* [Install Software](../../basic/install-osdeploysoftware.md)
* [7-Zip reference](../../../core-components/utilities/7zip.md)

# 7-Zip

7-Zip is an open-source archive utility used by OSDeploy in two roles: it is installed on the Windows 11 build machine via winget, and a standalone executable (`7zr.exe`) is injected directly into WinPE so that it is available at deployment time.

{% hint style="info" %}
7-Zip is optional for standalone `Build-OSDeployBootMedia` workflows but is injected into WinPE automatically when running `Invoke-OSDeployMDT` at the `WIM` stage.
{% endhint %}

| Property     | Value                                                        |
|--------------|--------------------------------------------------------------|
| Publisher    | Igor Pavlov (open source — [7-zip.org](https://www.7-zip.org/)) |
| Version      | 26.00                                                        |
| winget ID    | `7zip.7zip`                                                  |
| Architecture | amd64                                                        |
| Platform     | Windows 11 (build machine) / WinPE (injected)               |
| Required by  | `Install-OSDeploySoftware`, `Invoke-OSDeployMDT`             |
| Status       | Current                                                      |

{% embed url="https://github.com/ip7z/7zip/releases" %}
{% endembed %}

## Overview

OSDeploy uses 7-Zip in two distinct contexts:

**Build machine installation** — Installed on the Windows 11 build host via `winget install 7zip.7zip`. This provides the full 7-Zip suite for extracting driver packs and other archives used in build workflows.

**WinPE injection** — The standalone `7zr.exe` (version 26.00) from the 7-Zip extras archive is copied into WinPE's `System32` directory during `Invoke-OSDeployMDT` (at the `WIM` stage). This makes `7zr.exe` available at the command line in WinPE without a full installation.

## Why OSDeploy Uses 7-Zip

- Provides archive extraction capability in WinPE where no default decompression utility exists for formats beyond ZIP
- Enables extraction of vendor driver packs (`.7z`, `.cab`) during deployment workflows
- Injected as a single-file standalone executable — no installation required in WinPE

## Install on the Build Machine

```powershell
Install-OSDeploySoftware -Name '7zip' -Force
```

Run `Install-OSDeploySoftware` without parameters to preview the install command before running it.

Manual install via winget:

```powershell
winget install --id 7zip.7zip -e -h --accept-source-agreements --accept-package-agreements
```

## WinPE Injection

When `Invoke-OSDeployMDT` runs at the `WIM` stage, it automatically downloads the 7-Zip extras archive and copies `7zr.exe` to the mounted WinPE image's `System32` directory. No manual action is required.

The injected binary (`7zr.exe`) is a reduced-functionality standalone build of 7-Zip that supports extraction but not compression.

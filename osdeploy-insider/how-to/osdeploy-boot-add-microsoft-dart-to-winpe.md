---
description: Add Microsoft Diagnostics and Recovery Toolset tools and configuration to an AMD64 OSDeploy WinPE image.
---

# OSDeploy Boot: Add Microsoft DaRT to WinPE

Use the `dart` build option to add Microsoft Diagnostics and Recovery Toolset (DaRT) content to an AMD64 OSDeploy WinPE image. OSDeploy obtains the tools from the local DaRT installation and obtains the remote-connection configuration from the local Microsoft Deployment Toolkit (MDT) installation.

{% hint style="warning" %}
Install Microsoft Desktop Optimization Pack on the OSDeploy workstation so DaRT has full functionality. OSDeploy does not download or install Microsoft DaRT. Use DaRT only when your organization is properly licensed to use Microsoft Desktop Optimization Pack.
{% endhint %}

## Requirements

Prepare the workstation before starting:

* Run PowerShell 7.6 or later as an administrator.
* Install the current OSDeploy and OSDCloud modules.
* Install the Windows ADK and matching WinPE add-on.
* Update OSDeploy Core content.
* Build an `amd64` image. DaRT is not supported for `arm64` boot media.
* Install Microsoft Desktop Optimization Pack, including Microsoft DaRT 10.
* Install Microsoft Deployment Toolkit when the WinPE image requires the DaRT remote-connection configuration.

OSDeploy looks for these local source files:

```text
C:\Program Files\Microsoft DaRT\v10\Toolsx64.cab
C:\Program Files\Microsoft Deployment Toolkit\Templates\DartConfig8.dat
```

The DaRT tools CAB comes from Microsoft Desktop Optimization Pack. Without it, OSDeploy cannot add the DaRT tools. The MDT configuration is separate; without it, the tools can be added but `DartConfig.dat` is not placed in WinPE, so functionality that depends on that configuration is unavailable.

## Confirm the Local Sources

Check both source files before building:

```powershell
$DaRTTools = Join-Path $env:ProgramFiles 'Microsoft DaRT\v10\Toolsx64.cab'
$DaRTConfig = Join-Path $env:ProgramFiles `
    'Microsoft Deployment Toolkit\Templates\DartConfig8.dat'

[pscustomobject]@{
    DaRTToolsInstalled = Test-Path -LiteralPath $DaRTTools -PathType Leaf
    MDTConfigAvailable = Test-Path -LiteralPath $DaRTConfig -PathType Leaf
    DaRTToolsPath = $DaRTTools
    MDTConfigPath = $DaRTConfig
}
```

Both Boolean values should be `True` for the complete OSDeploy DaRT integration.

## Build WinPE with DaRT

Add DaRT to an interactive AMD64 build:

```powershell
Build-OSDeployBoot `
    -Architecture 'amd64' `
    -Options 'dart'
```

To use the Windows ADK WinPE image directly:

```powershell
Build-OSDeployBoot `
    -Architecture 'amd64' `
    -UseAdkWinPE `
    -Options 'dart'
```

Select the remaining shared content when prompted, then approve the build.

{% hint style="info" %}
OSDeploy does not add DaRT when the generated media name contains `public`, even when `dart` is selected and the cached tools are available.
{% endhint %}

## What the Build Adds

On the first eligible build, OSDeploy copies available source content into:

```text
C:\ProgramData\OSDeployCore\cache\winpe-apps\microsoft-dart
```

The cache can contain:

| Cached file | Source | Use |
| --- | --- | --- |
| `Toolsx64.cab` | Microsoft DaRT 10 from Microsoft Desktop Optimization Pack | Expanded into the root of the mounted AMD64 WinPE image. |
| `DartConfig.dat` | MDT `DartConfig8.dat` | Copied to `Windows\System32\DartConfig.dat` in WinPE. |

Existing cached files are reused. This permits later builds after the original source installation changes, but administrators remain responsible for licensing and for keeping the cached content current.

When `Toolsx64.cab` is missing from both the local installation and cache, the build reports that Microsoft Desktop Optimization Pack must be installed and continues without adding DaRT. When the MDT configuration is missing, the build reports that MDT must be installed and continues without `DartConfig.dat`.

## Verify DaRT in WinPE

Review the build output for these conditions:

* `Microsoft DaRT: Using cache content` confirms that the tools CAB was expanded.
* `Microsoft DaRT Config: Adding cache content` confirms that the MDT configuration was cached.
* A message about installing Microsoft Desktop Optimization Pack means the DaRT tools were not available.
* A message about installing Microsoft Deployment Toolkit means `DartConfig.dat` was not available.
* `Not supported for arm64 BootMedia` means the selected image architecture is incompatible.

Boot the generated ISO in an isolated test environment and confirm that the required DaRT tools and remote-connectivity workflow operate as expected before using the media for recovery.

See [Build-OSDeployBoot](../../guide/cmdlets/build-osdeployboot.md) for complete build behavior and [Microsoft MDT](../../guide/cmdlets/install-osdeploysoftware/microsoft-mdt.md) for the OSDeploy MDT installer workflow.

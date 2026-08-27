# Install-OSDeployMDT

Initializes an MDT Deployment Share for use with OSDeploy.

| Property | Value                                                   |
|----------|---------------------------------------------------------|
| Module   | OSDeploy                                                |
| Platform | Windows                                                 |
| Requires | MDT installed, Run as Administrator                     |

## Description

Configures an MDT Deployment Share so that `Invoke-OSDeployMDT` runs automatically on every **Update Deployment Share** operation.

Without `-Force`, the function runs in **audit mode** only — it reports the current state of the deployment share without making any changes.

With `-Force`, the function performs the following initialization steps:

1. Resolves the MDT installation directory and active Deployment Share path.
2. Creates `Templates\`, `Templates\winpe-drivers\`, and `Templates\winpe-extrafiles\` under the Deployment Share root.
3. Copies `winpeshl.ini`, `Wimscript.ini`, `Unattend_PE_x64.xml`, and `LiteTouchPE.xml` from the MDT templates directory into `DEPLOYROOT\Templates\`.
4. Copies `Background.bmp` from MDT Samples to `DEPLOYROOT\Templates\background.bmp`.
5. Rewrites `%INSTALLDIR%` references in `LiteTouchPE.xml` to use `%DEPLOYROOT%`.
6. Mirrors the `<Components>` element from `DEPLOYROOT\Boot\LiteTouchPE_x64.xml` into the template.
7. Registers `Invoke-OSDeployMDT` as the MDT LiteTouchPE exit command.
8. Updates `Control\Settings.xml`: disables x86, sets scratch space to 512 MB, sets wallpaper path.

Supports `-WhatIf` and `-Confirm`.

{% hint style="warning" %}
This function only needs to be run once per deployment share. Running it again with `-Force` will overwrite existing template files.
{% endhint %}

## Syntax

```powershell
Install-OSDeployMDT [-Force] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type     | Required | Description                                                                                                    |
|-----------|----------|----------|----------------------------------------------------------------------------------------------------------------|
| `-Force`  | `Switch` | No       | Performs all initialization changes. Without this switch the function runs in audit mode and reports state only. |

## Examples

```powershell
# Audit the current deployment share state without making changes
Install-OSDeployMDT
```

```powershell
# Initialize (or reinitialize) the deployment share
Install-OSDeployMDT -Force
```

```powershell
# Preview what changes would be made without executing them
Install-OSDeployMDT -Force -WhatIf
```

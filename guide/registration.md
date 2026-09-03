---
description: Register an OSDeploy PC with a free Recast Software Community License.
---

# Community Registration

Download and import a free Recast Software Community License, then use `Show-OSDeployLicense` to confirm that OSDeploy can discover and validate it. Complete registration before using the OSDeploy module.

{% hint style="warning" %}
A valid Recast Software Community License is required while the OSDeploy module is in preview. This requirement applies to OSDeploy on the PC used to create boot media. It does not apply to standalone use of the OSDCloud or legacy OSD modules in WinPE.
{% endhint %}

## Register the OSDeploy PC

{% stepper %}
{% step %}
### Download the Community License

1. Open the [Recast Software Community Portal](https://portal.recastsoftware.com/).
2. Create an account or sign in.
3. Download the license ZIP for **Right Click Tools Community Edition**.
{% endstep %}

{% step %}
### Import the License

Open PowerShell 7.6 or later as an administrator. Set `$LicenseFile` to the downloaded ZIP or an extracted `.license2` file, then import the license:

```powershell
$LicenseFile = 'C:\Path\To\CommunityLicense.zip'
Import-OSDeployLicense -LicenseFile $LicenseFile
```

{% hint style="warning" %}
Run `Import-OSDeployLicense` as an administrator. The function requires write access to the Recast Software license directory under `ProgramData`.
{% endhint %}

For ZIP input, the function extracts the archive and selects a contained `.license2` file. It validates the selected license, displays the proposed changes, and asks for confirmation before importing it. After a successful import, it displays the installed license using `Show-OSDeployLicense`.
{% endstep %}

{% step %}
### Verify the License

Verify the installed license:

```powershell
Show-OSDeployLicense
```

For detailed discovery and validation behavior, see [Show-OSDeployLicense](cmdlets/show-osdeploylicense.md).
{% endstep %}
{% endstepper %}

After `Show-OSDeployLicense` returns a valid license, continue to [Install Required Software](basic/install-osdeploysoftware.md).

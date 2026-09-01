---
description: Register an OSDeploy PC with a free Recast Software Community License.
---

# Community Registration

Download and install a free Recast Software Community License, then use `Show-OSDeployLicense` to confirm that OSDeploy can discover and validate it.

{% hint style="info" %}
Registration is optional. OSDeploy and OSDCloud work without a Community License, but some features are available only when a valid license is detected. Licensed features can change as the modules are updated.
{% endhint %}

## Register the OSDeploy PC

{% stepper %}
{% step %}
### Download the Community License

1. Open the [Recast Software Community Portal](https://portal.recastsoftware.com/).
2. Create an account or sign in.
3. Download the license ZIP for **Right Click Tools Community Edition**.
4. Extract the ZIP and locate the file with the `.license2` extension.
{% endstep %}

{% step %}
### Install the License

Open PowerShell 7.6 or later as an administrator. Set `$LicenseFile` to the extracted `.license2` file, then run the following commands:

```powershell
$LicenseFile = 'C:\Path\To\CommunityLicense.license2'
$LicenseDirectory = Join-Path -Path $env:ProgramData -ChildPath 'Recast Software\Licenses'

if (-not (Test-Path -LiteralPath $LicenseFile -PathType Leaf)) {
	throw "The license file was not found at $LicenseFile."
}

New-Item -Path $LicenseDirectory -ItemType Directory -Force | Out-Null
Copy-Item -LiteralPath $LicenseFile -Destination $LicenseDirectory -Force
```

{% hint style="warning" %}
Keep the `.license2` extension unchanged. OSDeploy does not discover license files with a different extension.
{% endhint %}
{% endstep %}

{% step %}
### Verify the License

Confirm that OSDeploy discovers the license and that its schema is valid:

```powershell
$License = Show-OSDeployLicense

if (-not $License) {
	throw 'OSDeploy did not find a usable Community License.'
}

if (-not $License.IsValid) {
	throw "The Community License failed validation: $($License.ValidationErrors -join '; ')"
}

$License | Select-Object FileName, Organization, Email, LicenseType, Expiration, ActivationExpiration
```

Confirm that the expiration dates have not passed. For detailed discovery and validation behavior, see [Show-OSDeployLicense](cmdlets/show-osdeploylicense.md).
{% endstep %}
{% endstepper %}

Continue to [Install Required Software](basic/install-osdeploysoftware.md), or continue without registration using the features available for unregistered use.

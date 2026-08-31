---
description: Register an OSDeploy PC with a free Recast Software Community License.
---

# Community Registration

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

Open PowerShell 7 as an administrator. Set `$LicenseFile` to the extracted `.license2` file, then run the following commands:

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
### Verify the License File

Confirm that the license file exists in the directory used by OSDeploy and OSDCloud:

```powershell
$LicenseDirectory = Join-Path -Path $env:ProgramData -ChildPath 'Recast Software\Licenses'
$LicenseFiles = Get-ChildItem -Path $LicenseDirectory -Filter '*.license2' -File

if (-not $LicenseFiles) {
	throw "No Community License file was found in $LicenseDirectory."
}

$LicenseFiles | Select-Object Name, DirectoryName, LastWriteTime
```
{% endstep %}
{% endstepper %}

The OSDeploy PC is now registered. Continue to [Install Software](../basic/install-osdeploysoftware.md), or continue without registration using the features available for unregistered use.

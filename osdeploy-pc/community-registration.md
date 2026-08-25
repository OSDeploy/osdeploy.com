# Registration

Registration is optional. OSDeploy and OSDCloud can be used without registering the OSDeploy PC.

Register the OSDeploy PC with a free Recast Software Community License to enable features reserved for registered use.

{% hint style="info" %}
Some OSDeploy and OSDCloud features are enabled only when a valid Recast Software Community License is detected. The available features can change as the modules are updated.
{% endhint %}

## Obtain a Community License

1. Open the [Recast Software Community Portal](https://portal.recastsoftware.com/).
2. Register for an account or sign in with an existing account.
3. Download the license ZIP for **Right Click Tools Community Edition**.

The ZIP contains a file with the `.license2` extension. OSDeploy discovers Community License files in `$env:ProgramData\Recast Software\Licenses`.

## Install the License

Open PowerShell 7 as an administrator. Set `$LicenseZip` to the full path of the ZIP downloaded from the Community Portal, then run the following commands:

```powershell
$LicenseZip = 'C:\Path\To\DownloadedLicense.zip'
$LicenseDirectory = Join-Path $env:ProgramData 'Recast Software\Licenses'
$ExtractDirectory = Join-Path $env:TEMP 'RecastCommunityLicense'

if (-not (Test-Path -Path $LicenseZip -PathType Leaf)) {
	throw "The license ZIP was not found at $LicenseZip."
}

Remove-Item -Path $ExtractDirectory -Recurse -Force -ErrorAction SilentlyContinue
New-Item -Path $LicenseDirectory -ItemType Directory -Force | Out-Null
New-Item -Path $ExtractDirectory -ItemType Directory -Force | Out-Null
Expand-Archive -Path $LicenseZip -DestinationPath $ExtractDirectory -Force

$LicenseFiles = Get-ChildItem -Path $ExtractDirectory -Filter '*.license2' -File -Recurse
if (-not $LicenseFiles) {
	throw 'The downloaded ZIP does not contain a .license2 file.'
}

$LicenseFiles | Copy-Item -Destination $LicenseDirectory -Force
Remove-Item -Path $ExtractDirectory -Recurse -Force
```

{% hint style="warning" %}
Keep the `.license2` file extension unchanged. OSDeploy does not discover renamed files with a different extension.
{% endhint %}

## Verify the License File

Confirm that at least one Community License file exists in the expected directory:

```powershell
$LicensePath = Join-Path $env:ProgramData 'Recast Software\Licenses\*.license2'

if (-not (Test-Path -Path $LicensePath -PathType Leaf)) {
	throw "No Community License file was found at $LicensePath."
}

Get-ChildItem -Path $LicensePath | Select-Object Name, DirectoryName, LastWriteTime
```

The OSDeploy PC is now registered and ready for OSDeploy Core initialization and OSDCloud boot-image creation. If registration is skipped, continue with the OSDeploy PC setup using the features available for unregistered use.

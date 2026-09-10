---
description: Import and reconcile a Recast Software license from a license file or ZIP archive.
---

# Import-OSDeployLicense

`Import-OSDeployLicense` validates a `.license2` file or ZIP archive, reconciles it with installed Recast Software licenses, and copies the selected license to the standard license directory.

## Requirements

Run the command in a PowerShell 7.6 or later session with write access to:

```text
C:\ProgramData\Recast Software\Licenses
```

The `-LicenseFile` path must identify an existing `.license2` file or a ZIP archive containing at least one `.license2` file.

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-LicenseFile` | `String` | None | Mandatory. Specify an existing `.license2` file or ZIP archive. ZIP archives are searched recursively. |
| `-WhatIf` | Common parameter | Not enabled | Display the proposed destination-directory, duplicate-removal, and copy operations without performing those `ShouldProcess` actions. Validation and the separate import confirmation still occur. |
| `-Confirm` | Common parameter | Not enabled | Request confirmation for operations controlled by `ShouldProcess`. The command also presents a separate import confirmation with `ShouldContinue`. |

## Examples

### Import a license file

Validate a license file, confirm the import, and copy it to the standard license directory:

```powershell
Import-OSDeployLicense -LicenseFile 'C:\Downloads\CommunityLicense.license2'
```

### Import a license from a ZIP archive

Extract the archive to a temporary directory, select a valid contained license, and confirm the import:

```powershell
Import-OSDeployLicense -LicenseFile 'C:\Downloads\CommunityLicense.zip'
```

### Preview filesystem changes

Run validation and confirmation while previewing the gated directory, removal, and copy operations:

```powershell
Import-OSDeployLicense `
    -LicenseFile 'C:\Downloads\CommunityLicense.license2' `
    -WhatIf
```

{% hint style="warning" %}
`-WhatIf` does not suppress the `ShouldContinue` import prompt. It prevents the later filesystem operations controlled by `ShouldProcess`.
{% endhint %}

## License Selection

A `.license2` input is staged and validated directly. For ZIP input, the command extracts the archive to a temporary directory and searches recursively for `.license2` files. When several candidates exist, the module license selector determines which valid candidate to import.

The command stops without changing the installed licenses when the input cannot be read, no license is found, validation fails, or the import confirmation is declined.

## Duplicate and Renewal Handling

The command compares the incoming license with installed candidates:

* Exact duplicate license files are removed.
* Licenses with the same license identity and renewal fingerprint are grouped together.
* The candidate with the newest expiration is retained.
* Existing files are not renamed.
* When the incoming filename collides with a different retained file, the incoming file receives a numeric suffix such as `-001`.

This reconciliation can remove older installed licenses before copying the selected incoming license.

## WhatIf and Confirmation

The command first uses `ShouldContinue` to confirm the selected license import. `-Force` is not available, so this prompt cannot be bypassed with a command parameter.

After approval, separate `ShouldProcess` calls control creation of the destination directory, removal of duplicate or superseded files, and copying the incoming license. `-Confirm` can prompt at those boundaries. A partially completed run is possible if an earlier operation is approved and a later operation is declined.

## Output

After a successful copy, the command calls `Show-OSDeployLicense` and returns its valid license `PSCustomObject`. Validation failures, declined operations, and unsuccessful copies return no license object.

See [Community Registration](../registration.md) for obtaining a license, [Show-OSDeployLicense](show-osdeploylicense.md) for license discovery behavior, or the [Import-OSDeployLicense command reference](../../command-reference/osdeploy/import-osdeploylicense.md) for compact syntax.

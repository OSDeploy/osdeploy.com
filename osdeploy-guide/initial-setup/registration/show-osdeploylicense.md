---
description: >-
  Discover, inspect, and validate the Recast Software license candidate selected
  for OSDeploy.
---

# Show-OSDeployLicense

`Show-OSDeployLicense` searches the standard Recast Software license directory and returns one selected `.license2` candidate as a PowerShell object. Use the returned properties to inspect its source file, identity, dates, authorized-command count, and schema validation results.

When no parseable candidate is available, the function writes a warning and displays Community License registration, installation, and verification instructions.

## Requirements

Run the function from PowerShell 7.6 or later on Windows with the current [OSDeploy module](../osdeploy-psmodule/) installed.

The function searches this directory:

```
C:\ProgramData\Recast Software\Licenses
```

A license file is not required to run the command. When the directory does not exist, license discovery attempts to create it. Reading an existing directory does not require an elevated session when the current account already has access.

{% hint style="warning" %}
Install Community License files with the `.license2` extension unchanged. Files with another extension are not discovered. Administrator rights can be required to create the ProgramData directory or copy a license into it.
{% endhint %}

See [Community Registration](./) to download and install a free Recast Software Community License.

## Parameters

`Show-OSDeployLicense` has no command-specific parameters and does not accept pipeline input. It does not support `-WhatIf` or `-Confirm` because it does not declare `SupportsShouldProcess`.

The command supports standard common parameters such as `-Verbose`, `-ErrorAction`, and `-WarningAction`.

## Examples

### Show the selected license

Discover the available `.license2` files and write the selected candidate to the pipeline:

```powershell
Show-OSDeployLicense
```

When no candidate can be selected, the command displays registration guidance and returns no object.

### Capture the selected license

Store the selected candidate and display its primary identity and status fields:

```powershell
$license = Show-OSDeployLicense

$license | Format-List `
	FileName, `
	Organization, `
	Email, `
	LicenseType, `
	Expiration, `
	ActivationExpiration, `
	IsValid
```

`$license` is `$null` when discovery does not produce a parseable candidate.

### Check validation errors

Inspect the validation result before relying on the selected candidate:

```powershell
$license = Show-OSDeployLicense

if (-not $license) {
	Write-Warning 'No license candidate was selected.'
}
elseif (-not $license.IsValid) {
	$license.ValidationErrors
}
```

`IsValid` reports the schema checks performed during discovery. It does not indicate that the expiration dates are current.

### Check expiration dates

Evaluate expiration and activation expiration independently of schema validation:

```powershell
$license = Show-OSDeployLicense
$now = Get-Date

[pscustomobject]@{
	SchemaValid      = $license.IsValid
	Expired          = $license.Expiration -and $license.Expiration -lt $now
	ActivationExpired = $license.ActivationExpiration -and $license.ActivationExpiration -lt $now
}
```

The command can select and return an expired candidate. It does not calculate `Expired` or `ActivationExpired` properties.

### Identify the selected source file

Review the exact file used for the returned candidate:

```powershell
$license = Show-OSDeployLicense

$license | Select-Object FileName, FullName, LastWriteTime, LicenseGuid
```

Use `-Verbose` to display discovered file counts, skipped unreadable files, duplicate handling, and the selected file:

```powershell
Show-OSDeployLicense -Verbose
```

## License Discovery

The command delegates discovery to `Get-OSDCoreLicense` with its default path. Discovery performs these operations:

1. Create `C:\ProgramData\Recast Software\Licenses` when the directory does not exist.
2. Enumerate root-level files matching `*.license2`.
3. Sort files by `LastWriteTime` descending so newer files are processed first.
4. Read each file as raw text and parse its JSON payload.
5. Treat each object in a JSON array as a separate candidate.
6. Validate expected fields and normalize GUID and date values.
7. Deduplicate candidates.
8. Rank the remaining candidates by email, modification time, and file name.
9. Return the first ranked candidate.

Discovery is not recursive. A `.license2` file in a child directory is not considered.

Unreadable files and files containing invalid JSON are skipped with verbose messages. A parseable entry can remain eligible even when required fields are missing or malformed; those conditions are recorded in `ValidationErrors`.

## Validation

Discovery checks each parseable entry for these conditions:

| Check                                                       | Validation error                                                |
| ----------------------------------------------------------- | --------------------------------------------------------------- |
| Top-level `Data` property exists                            | `Missing Data object`                                           |
| Top-level `Signature` is present and nonempty               | `Missing Signature`                                             |
| `Data.LicenseGuid` is present and parses as a GUID          | `Missing Data.LicenseGuid` or `Invalid Data.LicenseGuid format` |
| `Data.AuthorizedPluginCommands` contains at least one value | `Missing or empty Data.AuthorizedPluginCommands`                |
| Nonempty `Expiration` parses as `System.DateTime`           | `Invalid Expiration date format`                                |
| Nonempty `ActivationExpiration` parses as `System.DateTime` | `Invalid ActivationExpiration date format`                      |

`IsValid` is `$true` only when no validation errors are recorded.

{% hint style="warning" %}
`IsValid` describes these structural and conversion checks only. The discovery code hashes the signature for identity and deduplication but does not cryptographically verify it. It also does not reject a candidate whose `Expiration` or `ActivationExpiration` is in the past.
{% endhint %}

Use the returned dates and validation details together when evaluating the selected candidate.

## Duplicate Handling

Candidates are deduplicated in this order:

1. Use the normalized `LicenseGuid` when it is valid.
2. Otherwise, use the SHA256 hash of the nonempty signature.
3. Otherwise, use the source file path.

The first candidate for an identity is retained. Because files are processed newest-first, a duplicate in a newer file normally takes precedence over the same identity in an older file. Multiple entries within one payload retain their JSON order.

`SignatureHash` is a lowercase SHA256 hash of the signature text. It supports identity comparison without returning the signature itself.

## Candidate Selection

`Show-OSDeployLicense` does not accept a preferred email. After deduplication, it applies this public selection precedence:

1. Select a candidate with a nonempty email other than `support@recastsoftware.com`.
2. Fall back to `support@recastsoftware.com`.
3. Fall back to an empty or null email.

Within the same email rank, select the candidate with the newest source-file `LastWriteTime`. When timestamps match, sort by `FileName` ascending.

Selection does not filter on `IsValid`, `Expiration`, `ActivationExpiration`, `LicenseType`, or authorized-command count. A higher-ranked invalid or expired candidate can therefore be returned before a lower-ranked current candidate.

## No-Candidate Behavior

The function returns no object when:

* The license directory cannot be created or accessed.
* No root-level `.license2` files exist.
* Every discovered file is unreadable or contains invalid JSON.
* Parsing produces no candidate entries.

In this path, `Show-OSDeployLicense` writes a warning and displays instructions to:

1. Open the Recast Software Community Portal.
2. Download the Right Click Tools Community Edition license ZIP.
3. Extract and copy the `.license2` file into the standard license directory.
4. Verify the file with `Get-ChildItem`.

The guidance is host output, not a structured return object.

## Side Effects

The command can create `C:\ProgramData\Recast Software\Licenses` when it does not exist. Failure to create the directory is handled as a no-candidate result.

The command does not copy, rename, modify, or delete license files. It does not update `$global:OSDCoreLicense`; that global registration state is initialized separately by OSDeploy workflows that call `Initialize-OSDCoreLicense`.

## Output

When a candidate is selected, the command returns a `System.Management.Automation.PSCustomObject` with these properties:

| Property                       | Description                                                                                                            |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `FileName`                     | Leaf name of the source `.license2` file.                                                                              |
| `FullName`                     | Full path to the source file.                                                                                          |
| `LastWriteTime`                | Source file modification time used for tie-breaking.                                                                   |
| `LicenseGuid`                  | Normalized GUID string, or `$null` when missing or invalid.                                                            |
| `Organization`                 | Organization value from `Data`, or `$null` when absent.                                                                |
| `FirstName`                    | First-name value from `Data`, or `$null` when absent.                                                                  |
| `LastName`                     | Last-name value from `Data`, or `$null` when absent.                                                                   |
| `Email`                        | Email value from `Data`, or `$null` when absent.                                                                       |
| `LicenseType`                  | License-type value from `Data`, or `$null` when absent.                                                                |
| `DeviceCount`                  | Device-count value from `Data`, or `$null` when absent.                                                                |
| `UserCount`                    | User-count value from `Data`, or `$null` when absent.                                                                  |
| `Expiration`                   | Parsed `System.DateTime`, or `$null` when absent or empty. An invalid nonempty value also produces a validation error. |
| `ActivationExpiration`         | Parsed `System.DateTime`, or `$null` when absent or empty. An invalid nonempty value also produces a validation error. |
| `AuthorizedPluginCommandCount` | Number of entries in `Data.AuthorizedPluginCommands`; `0` when absent or empty.                                        |
| `SignatureHash`                | Lowercase SHA256 hash of the signature text, or `$null` when the signature is absent or empty.                         |
| `IsValid`                      | `$true` when the candidate has no recorded schema validation errors.                                                   |
| `ValidationErrors`             | Array of validation-error strings; empty when `IsValid` is `$true`.                                                    |
| `Data`                         | Original nested `Data` object from the parsed candidate, or `$null` when absent.                                       |

When no candidate is selected, the function returns no object and an assignment receives `$null`.

See [Community Registration](./) to download, install, and verify a Community License.

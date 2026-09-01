---
description: >-
  Understand how OSDeploy uses the Windows ESD catalog to select, download,
  cache, and verify Windows 11 media.
---

# Windows ESD Catalogs

This article steps through the code behind `Update-OSDeployCoreESD` to show how OSDeploy turns a Windows ESD catalog into a verified file in the local cache.

The catalog is the function's source of truth. It connects a Windows release, language, edition, and architecture to a specific ESD file on the Microsoft Content Delivery Network.

{% hint style="warning" %}
The bundled catalog may not contain the newest Windows release available from Microsoft. It contains the latest OS that has been tested with OSDeploy and verified to work. This favors a known working deployment source over unverified media simply because it is newer.
{% endhint %}

The catalog is included with the OSDeploy module under:

```
OSDeploy\core\operatingsystems\
```

Each XML file describes one Windows release. The catalog file name contains the Windows build and release:

```
26200.8653-win11-25h2.xml
```

| File name segment | Meaning                    |
| ----------------- | -------------------------- |
| `26200.8653`      | Windows build and revision |
| `win11`           | Windows 11                 |
| `25h2`            | Windows 11, version 25H2   |

Catalog names change when newer media is tested and added to OSDeploy. The function sorts the bundled catalog files by name and selects the first one.

{% hint style="info" %}
The catalog contains metadata and Microsoft download URLs. It does not contain the Windows installation media itself.
{% endhint %}

## Follow the Code Path

The function handles the catalog in three stages before a download begins:

1. Resolve and parse the latest bundled catalog.
2. Build target records and match them against the catalog.
3. Check the cache and URL before asking the user to download.

After confirmation, a separate download loop resumes transfers, handles retries, and verifies SHA256. Keeping discovery separate from transfer allows the function to evaluate every requested architecture before downloading any file.

## Resolve the Catalog

The function starts from `$script:OSDeployModuleBase`, which points to the installed module. It enumerates the XML files, sorts their names in descending order, and separates the first catalog from the older catalogs:

```powershell
$catalogDir  = Join-Path $script:OSDeployModuleBase 'core\operatingsystems'
$allXmlFiles = Get-ChildItem -Path $catalogDir -Filter '*.xml' -File |
	Sort-Object Name -Descending
$latestXml   = $allXmlFiles | Select-Object -First 1
$olderXmls   = @($allXmlFiles | Select-Object -Skip 1)
```

The file naming convention makes this sort meaningful because the build and revision appear first. If the directory contains no XML files, the function throws a terminating `CatalogNotFound` error instead of continuing without trusted metadata.

The selected XML is then cast directly to an XML document. The expression assigned to `$allFiles` moves through the document hierarchy and returns every `File` element:

```powershell
[xml]$catalog = Get-Content -Path $latestXml.FullName -Raw
$allFiles = $catalog.MCT.Catalogs.Catalog.PublishedMedia.Files.File
```

## Catalog Structure

The XML hierarchy leads to a collection of `File` elements:

```
MCT
└── Catalogs
	└── Catalog
		└── PublishedMedia
			└── Files
				└── File
```

Each `File` element is a logical media record. The fields used by OSDeploy are:

| Element        | Purpose                                      |
| -------------- | -------------------------------------------- |
| `FileName`     | Name used for the cached ESD                 |
| `LanguageCode` | Windows language and region, such as `en-us` |
| `Language`     | Display name for the language                |
| `Edition`      | Windows edition represented by the record    |
| `Architecture` | Media architecture: `x64` or `ARM64`         |
| `Size`         | File size in bytes                           |
| `Sha256`       | SHA256 checksum used to verify the ESD       |
| `FilePath`     | Microsoft Content Delivery Network URL       |

An abbreviated Enterprise record looks like this:

```xml
<File id="">
  <FileName>26200.8653...CLIENTBUSINESS_VOL_x64FRE_en-us.esd</FileName>
  <LanguageCode>en-us</LanguageCode>
  <Language>English (United States)</Language>
  <Edition>Enterprise</Edition>
  <Architecture>x64</Architecture>
  <Size>5871858284</Size>
  <Sha256>727d254e...858f1d33</Sha256>
  <FilePath>http://dl.delivery.mp.microsoft.com/.../media.esd</FilePath>
</File>
```

The same physical ESD can appear in several logical records. Microsoft business or consumer media can contain multiple Windows editions, so records for different editions can share the same file name, URL, size, and checksum. Select by the requested edition rather than assuming every record points to a unique file.

## Build the Target List

The catalog contains far more media than `Update-OSDeployCoreESD` needs. The function first creates small target objects that describe the records it wants. On AMD64 Windows, that target list is equivalent to:

```powershell
$targets = @(
	[pscustomobject]@{
		Architecture = 'x64'
		Edition = 'Enterprise'
		LanguageCode = 'en-us'
	}
	[pscustomobject]@{
		Architecture = 'ARM64'
		Edition = 'Enterprise'
		LanguageCode = 'en-us'
	}
)
```

On ARM64 Windows, only the ARM64 object is created. The public parameter uses `amd64`, but the catalog uses `x64`, so an internal map translates between the two vocabularies:

```powershell
$archMap = @{ amd64 = 'x64'; arm64 = 'ARM64' }
$targets = @(
	$targets | Where-Object {
		$_.Architecture -eq $archMap[$Architecture]
	}
)
```

Wrapping the filtered result in `@(...)` keeps `$targets` array-shaped even when only one record remains.

## Match the Catalog Records

For each target, the function filters all catalog records by three fields:

```powershell
$entry = $allFiles | Where-Object {
	$_.LanguageCode -eq $target.LanguageCode -and
	$_.Edition -eq $target.Edition -and
	$_.Architecture -eq $target.Architecture
} | Select-Object -First 1
```

`Select-Object -First 1` matters because one physical ESD can be represented by several logical edition records. The function needs one matching Enterprise record per requested architecture, not every record that refers to the same media.

| OSDeploy parameter    | Catalog value |
| --------------------- | ------------- |
| `-Architecture amd64` | `x64`         |
| `-Architecture arm64` | `ARM64`       |

The catalog can contain both architectures; the OSDeploy PC and `-Architecture` parameter control which records are eligible for selection.

Only the first matching record is selected for each architecture. If no matching Enterprise en-US record exists, the command writes a warning and skips that architecture.

## Derive the Cache Directory

The function does not hard-code `Windows 11 25H2`. It parses that label from the selected catalog name:

```powershell
$catalogBase = [System.IO.Path]::GetFileNameWithoutExtension($latestXml.Name)

if ($catalogBase -notmatch '^\d+\.\d+-win(\d+)-(.+)$') {
	# Throw CatalogNameUnrecognized
}

$osFolderName = "Windows $($Matches[1]) $($Matches[2].ToUpper())"
$downloadDir = Join-Path $script:OSDeployCorePath 'OSDCloud' 'OS' $osFolderName
```

For `26200.8653-win11-25h2.xml`, the capture groups contain `11` and `25h2`, producing:

```
C:\ProgramData\OSDeployCore\OSDCloud\OS\Windows 11 25H2\
```

The build revision is intentionally absent from the directory. New tested media for the same Windows release uses the same folder but has a different ESD file name.

## Phase 1: Classify Each Entry

The first processing loop does not immediately download anything. It classifies every resolved entry into one of these outcomes:

| Condition                              | Result                                          |
| -------------------------------------- | ----------------------------------------------- |
| Current file exists and SHA256 matches | Add it directly to `$results`                   |
| Current file exists and SHA256 differs | Offer to recycle it and continue                |
| Verified older file exists             | Offer to keep it or use the newer catalog entry |
| URL cannot be reached                  | Warn and skip the entry                         |
| URL is reachable                       | Add the entry to `$pendingEntries`              |

The expected hash is normalized before comparison:

```powershell
$normalizeHash = {
	param([string]$hash)
	($hash -replace '\s+', '').ToUpperInvariant()
}

$expectedSha256 = & $normalizeHash $entry.Sha256
$actualHash = (Get-FileHash -Path $destPath -Algorithm SHA256).Hash.ToUpperInvariant()
```

Removing whitespace and normalizing case prevents formatting differences from changing the checksum comparison.

Older catalogs are not fallback download sources. The function reads them only to identify an older file already present in the current release folder. That file is accepted only when its SHA256 matches its own catalog record.

Before an uncached entry reaches the confirmation prompt, `curl.exe` sends a HEAD request with a 15-second limit:

```powershell
$null = curl.exe --head --fail --silent --location --max-time 15 $entry.FilePath 2>&1

if ($LASTEXITCODE -eq 0) {
	$pendingEntries.Add($entry)
}
```

This prevents an unavailable catalog URL from being presented as a viable download.

## Phase 2: Confirm Before Transfer

The function loops through `$pendingEntries`, calculates a display size from the catalog, and asks for confirmation. Approved entries move into `$confirmedEntries`:

```powershell
$fileSizeMB = [Math]::Round([long]$entry.Size / 1MB, 1)

if (
	$PSCmdlet.ShouldContinue($confirmMessage, $confirmCaption) -and
	$PSCmdlet.ShouldProcess($destPath, "Download $($entry.FileName)")
) {
	$confirmedEntries.Add($entry)
}
```

`ShouldContinue` provides the interactive Yes/No choice. `ShouldProcess` integrates the operation with `-WhatIf`. No transfer begins until all pending entries have passed through this phase.

## Phase 3: Download and Verify

The download loop first requests `Content-Length` and `Accept-Ranges` from the server. Those headers determine whether the function can detect an incomplete resumable transfer by comparing local and remote byte counts.

The `curl.exe` arguments enable redirects, HTTP failure handling, internal curl retries, and continuation of a partial file:

```powershell
$curlArgs = @(
	'--location', '--fail', '--retry', '5',
	'--user-agent', 'Mozilla/5.0 ...',
	'--continue-at', '-',
	'--output', $destPath,
	$entry.FilePath
)
```

Around curl's own retry behavior, the function has an outer retry loop with three automatic attempts. Each attempt is classified as one of four failures:

| Failure              | Code condition                                    |
| -------------------- | ------------------------------------------------- |
| `DownloadFailed`     | `curl.exe` returns a nonzero exit code            |
| `DownloadMissing`    | curl returns success but no output file exists    |
| `DownloadIncomplete` | resumable local length is below the remote length |
| `ChecksumMismatch`   | local SHA256 differs from the catalog             |

A matching SHA256 sets `$downloadSucceeded` to `$true`. Only then is the `FileInfo` added to `$results`. After automatic attempts are exhausted, the function reports the relevant details and offers a manual retry.

Checksum failures are handled differently from incomplete transfers. A partial file can be resumed, but a complete file with the wrong checksum is moved to the Recycle Bin so the next attempt starts cleanly.

{% hint style="warning" %}
Do not treat a familiar file name or a completed download as proof that an ESD is valid. The file is ready only after its SHA256 matches the same catalog record that supplied its URL.
{% endhint %}

## Inspect the Installed Catalog

Import OSDeploy, locate the latest catalog, and load it as XML:

```powershell
Import-Module OSDeploy

$CatalogDirectory = Join-Path (Get-Module OSDeploy).ModuleBase 'core\operatingsystems'
$CatalogFile = Get-ChildItem -Path $CatalogDirectory -Filter '*.xml' -File |
	Sort-Object Name -Descending |
	Select-Object -First 1

[xml]$Catalog = Get-Content -Path $CatalogFile.FullName -Raw
$CatalogFiles = $Catalog.MCT.Catalogs.Catalog.PublishedMedia.Files.File
```

Review the available architectures, languages, and editions:

```powershell
$CatalogFiles.Architecture | Sort-Object -Unique
$CatalogFiles.LanguageCode | Sort-Object -Unique
$CatalogFiles.Edition | Sort-Object -Unique
```

Display the records selected by OSDeploy:

```powershell
$CatalogFiles |
	Where-Object {
		$_.LanguageCode -eq 'en-us' -and
		$_.Edition -eq 'Enterprise'
	} |
	Select-Object FileName, Architecture, Size, Sha256, FilePath
```

Convert the byte count to GiB when reviewing media size:

```powershell
$CatalogFiles |
	Where-Object {
		$_.LanguageCode -eq 'en-us' -and
		$_.Edition -eq 'Enterprise'
	} |
	Select-Object Architecture, FileName, @{
		Name = 'SizeGiB'
		Expression = { [Math]::Round([long]$_.Size / 1GB, 2) }
	}
```

## Why the Catalog Is Bundled

Bundling the catalog with the module makes media selection deterministic. A particular OSDeploy module version carries a known set of ESD metadata, and the download is accepted only when it matches that metadata.

Updating the OSDeploy module can therefore update the available Windows media without changing the `Update-OSDeployCoreESD` command. The command supplies the selection and verification behavior; the catalog supplies the release-specific data.

## Related

* [Update Windows 11 ESD](../../guide/cmdlets/update-osdeploycoreesd.md)
* [Update-OSDeployCoreESD command reference](../../command-reference/osdeploy/update-osdeploycoreesd.md)
* [Update Windows 11 OS](../../guide/cmdlets/update-osdeploycoreos.md)

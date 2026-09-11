---
description: Understand the data stored and transmitted by the OSDeploy PowerShell module.
---

# OSDeploy Privacy

OSDeploy runs locally to maintain OSDeployCore content and create deployment media. It stores downloaded content, build profiles, logs, license information, and generated media on the computer where its commands run.

Effective date: September 10, 2026

## Deployment Analytics

`Build-OSDeployBoot` sends one `Build-OSDeployBoot` event to the PostHog capture endpoint at `https://us.i.posthog.com/capture/`.

The event contains:

* `BootGuid`, a new identifier generated for the build
* `Email`, the email from the selected local license, or `Unregistered`
* `LicenseGuid`, the GUID from the selected local license, or `Unregistered`
* `distinct_id`, an unsalted SHA-256 hash of the build computer SMBIOS UUID, or a generated GUID when no hash is produced
* The event name, PostHog project API key, and an ISO 8601 timestamp with the local time offset

The device hash is pseudonymous, not anonymous. OSDeploy does not include the raw SMBIOS UUID, complete license file, license signature, build logs, generated media contents, or downloaded artifacts in this event.

{% hint style="warning" %}
The event is sent after the build configuration is displayed and after the five-second cancellation period, but before PowerShell evaluates `ShouldProcess`. Using `-WhatIf` or declining `-Confirm` does not prevent the event from being sent. Cancel the command during the five-second period to stop before the event is sent.
{% endhint %}

The HTTPS request has a two-second timeout and is not retried. Handled request or payload failures do not stop the build. OSDeploy does not currently provide a command parameter, environment variable, or configuration setting to opt out of this event.

## Generated Media Identity

OSDeploy can store these environment variables in generated WinPE:

| Variable | Content |
| --- | --- |
| `ID_OSDEPLOYDEVICE` | SHA-256 hash of the local SMBIOS UUID, or a generated GUID when no hash is available. |
| `ID_OSDEPLOYBOOT` | Identifier generated for the boot build. |
| `ID_REGISTEREDEMAIL` | Email address from the selected local Recast license when available. |
| `ID_REGISTEREDLICENSE` | GUID from the selected local Recast license when available. |

Generated media can also contain copied Recast license files from `%ProgramData%\Recast Software\Licenses`. These values remain local unless the operator shares, copies, boots, publishes, or distributes the media.

## Local Data

OSDeployCore data is stored under `%ProgramData%\OSDeployCore` by default. This includes:

* Boot images, ISO files, and build metadata under `boot`
* Shared profiles, drivers, scripts, startup profiles, and wallpapers under `boot-assets`
* Downloads, Windows images, recovery images, app payloads, hashes, and configuration under `cache`
* Transcripts, DISM output, driver logs, package logs, and other diagnostics
* Build selections and profile settings, including custom scripts and startup content

Temporary working content can also be created under `%TEMP%`. Recast license files are read from `%ProgramData%\Recast Software\Licenses`.

## License Data

OSDeploy reads local `.license2` files to identify and validate a selected Recast license. Parsed data can include the license GUID, organization, name, email, license type, device and user counts, expiration values, authorized commands, and a hash of the signature.

The current shared license gate applies to direct calls to:

* `Update-OSDeployBootISO`
* `Update-OSDeployBootProfilePreview`
* `Update-OSDeployCore`
* `Update-OSDeployCoreDrivers`
* `Update-OSDeployCoreESD`
* `Update-OSDeployCoreRE`

The Core update commands called immediately by `Invoke-OSDeployHydration` use its limited caller exception. `Build-OSDeployBoot` does not use the shared license gate, but selected license identity can be included in its analytics event and generated media.

## External Services

OSDeploy contacts external services when an operator chooses commands that download or refresh content, install software, or build media with downloaded applications. These services can include Microsoft download services, Microsoft Learn, PowerShell Gallery, GitHub, OEM driver sites, Windows Package Manager sources, the Recast Software Community Portal, PostHog, and Web Archive.

Those services can process normal network metadata such as the IP address, request headers, requested URL, and timestamp under their own privacy terms.

## Operator Choices

* Do not run `Build-OSDeployBoot` when the PostHog analytics event must not occur. There is no separate analytics opt-out.
* Do not run download, refresh, or installation commands when external network requests must be avoided.
* Review `%ProgramData%\OSDeployCore`, `%TEMP%`, and `%ProgramData%\Recast Software\Licenses` when managing local retention.
* Review generated ISOs, USB media, deployment shares, and logs before sharing them outside the organization.

For questions or concerns, open an issue in the [RecastOSDeploy repository](https://github.com/RecastSoftware/RecastOSDeploy/issues).

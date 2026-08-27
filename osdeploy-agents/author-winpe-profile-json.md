---
description: >-
  Agent instructions for creating and validating OSDCloud WinPE startup profile
  JSON files from the profiles included with OSDCloud.
icon: brackets-curly
---

# Author a WinPE Profile JSON

Use these instructions to create or update an OSDCloud WinPE startup profile for `Invoke-WinPEStartup`.

{% hint style="info" %}
Treat the profiles included with OSDCloud as the default starting points. Make the smallest change that satisfies the requested WinPE behavior.
{% endhint %}

## Agent contract

Create a flat JSON object that maps `Invoke-WinPEStartup` parameter names to values. Do not embed deployment workflow implementation or nested objects in the profile.

Follow these rules:

* Use the exact `Invoke-WinPEStartup:` prefix for every property.
* Use JSON booleans (`true` and `false`), not quoted boolean strings.
* Use a string for one module or command and an array for multiple ordered values.
* Preserve unrelated properties, ordering, and formatting when editing an existing profile.
* Do not add properties only to restate their default values.
* Use standard JSON without comments.
* Use a descriptive `.json` filename when creating a profile.

In the OSDCloud source, profiles are stored in `OSDCloud/core/OSDRepo/winpe-profiles/`. At runtime, `Invoke-WinPEStartup` discovers profiles in `WinPEStartup\Profiles` on attached drives. The function runs only in WinPE, where `$env:SystemDrive` is `X:`.

## Select a default profile

Inspect the existing profiles before creating a new file. Start with the profile whose behavior is closest to the request.

| Requested behavior | Default profile |
| --- | --- |
| Display device information and deploy Windows | `Deploy-OSDCloud.json` |
| Display device information, deploy Windows, and restart | `Deploy-OSDCloud Restart.json` |
| Display device information without deploying Windows | `Show-OSDCloudDeviceInfo.json` |

Create a new profile only when none of these defaults is an appropriate base.

### Deploy OSDCloud

Use this profile as the default for an interactive OSDCloud deployment:

{% code title="Deploy-OSDCloud.json" %}
```json
{
  "Invoke-WinPEStartup:InvokeMainCommand": [
    "Show-OSDCloudDeviceInfo",
    "Deploy-OSDCloud"
  ],
  "Invoke-WinPEStartup:InvokeMainCommandNoExit": true,
  "Invoke-WinPEStartup:InvokeMainCommandEA": "Continue"
}
```
{% endcode %}

### Deploy OSDCloud and restart

Use this profile when WinPE must restart after the deployment command finishes:

{% code title="Deploy-OSDCloud Restart.json" %}
```json
{
  "Invoke-WinPEStartup:InvokeMainCommand": [
    "Show-OSDCloudDeviceInfo",
    "Deploy-OSDCloud"
  ],
  "Invoke-WinPEStartup:InvokeMainCommandNoExit": true,
  "Invoke-WinPEStartup:InvokeMainCommandEA": "Continue",
  "Invoke-WinPEStartup:InvokeShutdownCommand": [
    "Restart-Computer -Force"
  ]
}
```
{% endcode %}

### Show device information

Use this profile for inspection without deployment:

{% code title="Show-OSDCloudDeviceInfo.json" %}
```json
{
  "Invoke-WinPEStartup:InvokeMainCommand": [
    "Show-OSDCloudDeviceInfo"
  ],
  "Invoke-WinPEStartup:InvokeMainCommandNoExit": true,
  "Invoke-WinPEStartup:InvokeMainCommandEA": "Continue"
}
```
{% endcode %}

The included profiles keep the main child PowerShell process open for operator interaction. Preserve `InvokeMainCommandNoExit: true` when extending one of these interactive profiles. For a fully unattended profile, omit the property or set it to `false`.

## Configuration reference

Every supported profile property is listed below. Switch parameters default to `false`, command and module parameters default to empty, and error-action parameters default to `Continue`.

### Startup options

| Property | JSON value | Effect |
| --- | --- | --- |
| `Invoke-WinPEStartup:SkipOnScreenKeyboard` | boolean | Skip the on-screen keyboard check. Default: `false`. |
| `Invoke-WinPEStartup:ShowPnpDevices` | boolean | Show the Plug and Play device hardware window. Default: `false`. |
| `Invoke-WinPEStartup:ShowPnpErrors` | boolean | Show the Plug and Play device error window. Default: `false`. |
| `Invoke-WinPEStartup:SkipWiFi` | boolean | Skip Wi-Fi startup and connection checks. Default: `false`. |
| `Invoke-WinPEStartup:SkipIPConfig` | boolean | Skip the IP configuration display. Default: `false`. |
| `Invoke-WinPEStartup:SkipUpdateOSDCloud` | boolean | Skip the OSDCloud module update. Default: `false`. |
| `Invoke-WinPEStartup:InstallModule` | string or string array | Install or update one or more additional PowerShell modules. Default: empty. |

### Startup command phase

| Property | JSON value | Effect |
| --- | --- | --- |
| `Invoke-WinPEStartup:InvokeStartupCommand` | string or string array | Run commands after connectivity and module updates but before the main phase. Default: empty. |
| `Invoke-WinPEStartup:InvokeStartupCommandNoExit` | boolean | Keep the startup child PowerShell process open after its commands finish. Default: `false`. |
| `Invoke-WinPEStartup:InvokeStartupCommandEA` | `Continue` or `Stop` | Continue startup with a warning or terminate startup when the child process fails. Default: `Continue`. |

### Main command phase

| Property | JSON value | Effect |
| --- | --- | --- |
| `Invoke-WinPEStartup:InvokeMainCommand` | string or string array | Run inspection, deployment, or other main commands. Default: empty. |
| `Invoke-WinPEStartup:InvokeMainCommandNoExit` | boolean | Keep the main child PowerShell process open after its commands finish. Default: `false`. |
| `Invoke-WinPEStartup:InvokeMainCommandEA` | `Continue` or `Stop` | Continue startup with a warning or terminate startup when the child process fails. Default: `Continue`. |

### Shutdown command phase

| Property | JSON value | Effect |
| --- | --- | --- |
| `Invoke-WinPEStartup:InvokeShutdownCommand` | string or string array | Run final commands after the main phase. Default: empty. |
| `Invoke-WinPEStartup:InvokeShutdownCommandNoExit` | boolean | Keep the shutdown child PowerShell process open after its commands finish. Default: `false`. |
| `Invoke-WinPEStartup:InvokeShutdownCommandEA` | `Continue` or `Stop` | Continue with a warning or terminate startup when the child process fails. Default: `Continue`. |

## Command behavior

Place each command in the phase that matches its purpose:

1. Use `InvokeStartupCommand` for preparation that must run after networking and module updates.
2. Use `InvokeMainCommand` for device inspection, deployment, and the primary task.
3. Use `InvokeShutdownCommand` for final actions such as restarting the device.

Commands in an array execute in array order in one child Windows PowerShell process. State created by an earlier command is therefore available to later commands in the same phase.

A command entry beginning with `http://` or `https://` is automatically converted to the following expression by `Invoke-WinPEStartup`:

```powershell
Invoke-RestMethod -Uri '<url>' | Invoke-Expression
```

Write the URL directly in the JSON profile. Do not wrap it in `Invoke-RestMethod` yourself.

Set a phase's `NoExit` property to `true` only when its child PowerShell window must remain open for interaction or troubleshooting. For normal unattended execution, omit it or use `false`.

Use `Continue` when a child-process failure should write a warning and allow startup to proceed. Use `Stop` when failure in that phase must terminate `Invoke-WinPEStartup`.

Use `Restart-Computer -Force` to restart at the end of the shutdown phase. Do not use `shutdown.exe` unless its specific options are required.

## Authoring workflow

{% stepper %}
{% step %}
### Identify the requested behavior

Determine whether the profile must change device display, networking, module updates, startup commands, main commands, shutdown commands, or error handling.
{% endstep %}

{% step %}
### Inspect the nearest default

Read the target profile when editing. For a new profile, select the closest included profile and preserve its established command order and formatting.
{% endstep %}

{% step %}
### Map behavior to properties

Use only properties from the configuration reference. Keep inspection and deployment in the main phase, and place restart behavior in the shutdown phase.
{% endstep %}

{% step %}
### Make the smallest change

Add or update only the required properties. Preserve unrelated values. Use arrays when multiple modules or commands must run in a defined order.
{% endstep %}

{% step %}
### Validate without execution

Parse the JSON and inspect changed properties. Do not run any command stored in the profile during validation.
{% endstep %}
{% endstepper %}

## Discovery and precedence

At runtime, `Invoke-WinPEStartup` scans drives `C:` through `Z:` for `WinPEStartup\Profiles\*.json`.

* One discovered profile is selected automatically.
* Multiple discovered profiles produce a selection menu.
* No discovered profiles allows startup to continue without a profile.

Explicit parameters passed to `Invoke-WinPEStartup` take precedence over JSON configuration. A selected profile overrides matching module JSON defaults. Function defaults apply when no higher-precedence value is provided.

Although the runtime loader accepts profile keys without the `Invoke-WinPEStartup:` prefix, always use the prefix. This keeps profiles consistent with the included defaults and makes each property's owner explicit.

## Validate the profile

Parse the profile with Windows PowerShell 5.1. Parsing confirms JSON syntax without starting deployment, downloading content, or restarting the device.

```powershell
$profilePath = '.\OSDCloud\core\OSDRepo\winpe-profiles\<profile>.json'
$profile = Get-Content -LiteralPath $profilePath -Raw | ConvertFrom-Json
$profile | Out-Null
```

Inspect every property changed by the request. For example:

```powershell
$profile.'Invoke-WinPEStartup:InvokeMainCommand'
$profile.'Invoke-WinPEStartup:InvokeShutdownCommand'
```

Before completing the profile, confirm that:

* The file contains one flat JSON object with no nested objects.
* Every property uses the exact `Invoke-WinPEStartup:` prefix and a supported name.
* Boolean values are unquoted.
* Every error-action value is `Continue` or `Stop`.
* Command and module arrays contain no empty entries.
* Command order matches the requested execution order.
* The validation process did not execute any profile command.

{% hint style="warning" %}
The runtime loader tolerates JSON comments, but standard JSON validation does not. Do not add comments to a profile.
{% endhint %}
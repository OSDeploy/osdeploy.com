---
description: >-
  Agent instructions for creating, editing, and validating WinPEStartup Profile
  JSON files for OSDeploy Boot and OSDCloud.
icon: brackets-curly
---

# WinPEStartup Profile Agent

Use these instructions to create, edit, or validate a WinPEStartup Profile JSON file that configures `Invoke-WinPEStartup`.

{% hint style="info" %}
Create user-authored profiles in `C:\ProgramData\OSDeployCore\boot-assets\winpestartup-profiles`. `Build-OSDeployBoot` can select profiles from this shared Boot-Assets library and copy them into the WinPE boot image.
{% endhint %}

## Agent contract

Determine whether the user wants to create, edit, or validate a profile. Extract the requested filename and behavior before changing a file.

Follow these rules:

* Proceed without questions when the request includes a profile name and enough behavioral detail.
* Ask one concise group of questions only when choices that materially affect the profile remain unresolved.
* Add the `.json` extension when the user omits it.
* Accept a filename only. Reject rooted paths, path separators, `.`, and `..`.
* Create or edit the JSON file directly. Do not use or recommend an OSDeploy function to author it.
* Do not write a profile into an installed PowerShell module directory.
* Use a flat JSON object with exact `Invoke-WinPEStartup:` property names.
* Use native JSON booleans and arrays of strings. Do not use quoted booleans, nested objects, comments, or unsupported properties.
* Omit unrequested properties so they inherit OSDCloud defaults.
* Do not emit empty arrays unless the user explicitly wants to clear an inherited collection.
* Read an existing profile before changing it and preserve unrelated supported properties and formatting where practical.
* Ask before replacing an existing profile unless the user explicitly requested replacement.
* For a validation-only request, do not modify the file.
* Never execute commands or URLs stored in a profile during validation.

## Authoring modes

Use **automatic mode** when the user supplies a profile name and enough detail to create or update it.

Use **guided mode** when required behavior is missing or ambiguous. Ask only about unresolved choices such as:

1. A descriptive profile filename.
2. Commands or URLs for the startup, main, and shutdown phases.
3. Additional PowerShell modules to install or update.
4. Skip or display settings.
5. Whether a child PowerShell process must remain open.
6. Whether command failure should continue startup or stop it.

Prefer selectable answers for fixed choices and free text for filenames, modules, commands, and URLs. Group related questions into one interaction.

Use these conservative defaults:

* Inherit omitted OSDCloud settings.
* Keep child PowerShell processes non-interactive by omitting `NoExit`.
* Use `Continue` unless a failed command must stop startup.
* Put preparation commands in the startup phase, deployment or inspection commands in the main phase, and restart commands in the shutdown phase.

## Profile destination

Create new profiles at:

```text
C:\ProgramData\OSDeployCore\boot-assets\winpestartup-profiles\<name>.json
```

Create the `winpestartup-profiles` directory when it does not exist.

An OSDeploy Boot profile can also contain JSON files in its local `WinPEStartup\profiles` directory. Those files are copied automatically during a build, but the shared Boot-Assets library is the standard destination for newly authored profiles.

## Configuration reference

| Property | JSON value | Effect |
| --- | --- | --- |
| `Invoke-WinPEStartup:SkipOnScreenKeyboard` | boolean | Skip the on-screen keyboard check. |
| `Invoke-WinPEStartup:ShowPnpDevices` | boolean | Show Plug and Play device hardware. |
| `Invoke-WinPEStartup:ShowPnpErrors` | boolean | Show Plug and Play device errors. |
| `Invoke-WinPEStartup:SkipWiFi` | boolean | Skip Wi-Fi startup and connection checks. |
| `Invoke-WinPEStartup:SkipIPConfig` | boolean | Skip the IP configuration display. |
| `Invoke-WinPEStartup:SkipUpdateOSDCloud` | boolean | Skip the OSDCloud module update. |
| `Invoke-WinPEStartup:InstallModule` | string array | Install or update additional PowerShell modules. |
| `Invoke-WinPEStartup:InvokeStartupCommand` | string array | Run preparation commands before the main phase. |
| `Invoke-WinPEStartup:InvokeStartupCommandNoExit` | boolean | Keep the startup child PowerShell process open. |
| `Invoke-WinPEStartup:InvokeStartupCommandEA` | `Continue` or `Stop` | Handle startup child-process failure. |
| `Invoke-WinPEStartup:InvokeMainCommand` | string array | Run deployment, inspection, or other primary commands. |
| `Invoke-WinPEStartup:InvokeMainCommandNoExit` | boolean | Keep the main child PowerShell process open. |
| `Invoke-WinPEStartup:InvokeMainCommandEA` | `Continue` or `Stop` | Handle main child-process failure. |
| `Invoke-WinPEStartup:InvokeShutdownCommand` | string array | Run final commands after the main phase. |
| `Invoke-WinPEStartup:InvokeShutdownCommandNoExit` | boolean | Keep the shutdown child PowerShell process open. |
| `Invoke-WinPEStartup:InvokeShutdownCommandEA` | `Continue` or `Stop` | Handle shutdown child-process failure. |

Apply these inheritance rules:

* An omitted property inherits the OSDCloud default.
* An explicit boolean overrides its inherited value.
* A string array replaces the inherited collection. An empty array intentionally clears it.
* `Continue` writes a warning and proceeds after a child-process failure.
* `Stop` raises a terminating error.
* Explicit `Invoke-WinPEStartup` arguments override profile values. Profile values override OSDCloud module defaults.

## Command behavior

Store each command, module, or URL as one array entry. Entries execute in order in one child Windows PowerShell process for that phase, so state created by an earlier command is available to later commands in the same phase.

Use the phase that matches the command's purpose:

1. Use `InvokeStartupCommand` for preparation after connectivity and module updates.
2. Use `InvokeMainCommand` for device inspection, deployment, and the primary task.
3. Use `InvokeShutdownCommand` for final actions such as restarting the device.

A command entry beginning with `http://` or `https://` is converted by `Invoke-WinPEStartup` to:

```powershell
Invoke-RestMethod -Uri '<url>' | Invoke-Expression
```

Write the URL directly in the profile. Do not wrap it in `Invoke-RestMethod`.

Use `Restart-Computer -Force` to restart after deployment. Use `shutdown.exe` only when its specific options are required.

Set a phase's `NoExit` property only when the user requests an interactive child PowerShell window. `Invoke-WinPEStartup` waits for that process to close.

## Example

For a profile named `Deploy-And-Restart`, create `C:\ProgramData\OSDeployCore\boot-assets\winpestartup-profiles\Deploy-And-Restart.json`:

{% code title="Deploy-And-Restart.json" %}
```json
{
  "Invoke-WinPEStartup:InvokeMainCommand": [
    "Show-OSDCloudDeviceInfo",
    "Deploy-OSDCloud"
  ],
  "Invoke-WinPEStartup:InvokeMainCommandEA": "Stop",
  "Invoke-WinPEStartup:InvokeShutdownCommand": [
    "Restart-Computer -Force"
  ]
}
```
{% endcode %}

## Build and runtime lifecycle

1. `Build-OSDeployBoot` includes JSON files from the shared user repository in its WinPEStartup profile selector.
2. Selected paths are saved in the build profile's `WinPEStartupProfile` property.
3. The build copies selected files into `WinPEStartup\profiles` in the mounted WinPE image.
4. In WinPE, `Invoke-WinPEStartup` scans drives `C:` through `Z:` for `<drive>:\WinPEStartup\profiles\*.json`.
5. One discovered profile is selected automatically. Multiple profiles produce a numbered selection prompt.
6. Cancelling the prompt or selecting a malformed profile stops the remaining startup sequence. A malformed profile also produces a warning.

## Authoring workflow

{% stepper %}
{% step %}
### Identify the operation

Determine whether the user wants to create, edit, or validate a profile. Extract the filename and requested behavior.
{% endstep %}

{% step %}
### Resolve required choices

Proceed automatically when the request is complete. Otherwise, ask one grouped set of questions about only the unresolved settings.
{% endstep %}

{% step %}
### Inspect the target

For an edit or replacement, read the existing profile first. Preserve unrelated supported properties and avoid replacing the file without approval.
{% endstep %}

{% step %}
### Write the smallest change

Use only supported canonical properties. Omit settings that should inherit OSDCloud defaults.
{% endstep %}

{% step %}
### Validate without execution

Parse and type-check the profile. Do not invoke any stored command or URL.
{% endstep %}

{% step %}
### Report the result

Report the profile path, validation result, and a concise summary of the configured behavior or changes.
{% endstep %}
{% endstepper %}

## Validate the profile

After creating or editing a profile, parse and type-check it with PowerShell. Use the same procedure for a validation-only request. This code reads profile values but never invokes a stored command:

```powershell
$profilePath = 'C:\ProgramData\OSDeployCore\boot-assets\winpestartup-profiles\Deploy-And-Restart.json'
$profile = Get-Content -LiteralPath $profilePath -Raw -ErrorAction Stop | ConvertFrom-Json -ErrorAction Stop

$booleanKeys = @(
    'Invoke-WinPEStartup:SkipOnScreenKeyboard'
    'Invoke-WinPEStartup:ShowPnpDevices'
    'Invoke-WinPEStartup:ShowPnpErrors'
    'Invoke-WinPEStartup:SkipWiFi'
    'Invoke-WinPEStartup:SkipIPConfig'
    'Invoke-WinPEStartup:SkipUpdateOSDCloud'
    'Invoke-WinPEStartup:InvokeStartupCommandNoExit'
    'Invoke-WinPEStartup:InvokeMainCommandNoExit'
    'Invoke-WinPEStartup:InvokeShutdownCommandNoExit'
)
$arrayKeys = @(
    'Invoke-WinPEStartup:InstallModule'
    'Invoke-WinPEStartup:InvokeStartupCommand'
    'Invoke-WinPEStartup:InvokeMainCommand'
    'Invoke-WinPEStartup:InvokeShutdownCommand'
)
$errorActionKeys = @(
    'Invoke-WinPEStartup:InvokeStartupCommandEA'
    'Invoke-WinPEStartup:InvokeMainCommandEA'
    'Invoke-WinPEStartup:InvokeShutdownCommandEA'
)
$supportedKeys = @($booleanKeys + $arrayKeys + $errorActionKeys)

if ($profile -isnot [pscustomobject] -or $profile -is [System.Array]) {
    throw 'The profile must contain one JSON object.'
}
if (@($profile.PSObject.Properties).Count -eq 0) {
    throw 'The profile must contain at least one explicit setting.'
}

foreach ($property in $profile.PSObject.Properties) {
    if ($property.Name -notin $supportedKeys) {
        throw "Unsupported profile property '$($property.Name)'."
    }
    if ($property.Name -in $booleanKeys -and $property.Value -isnot [bool]) {
        throw "'$($property.Name)' must be a JSON boolean."
    }
    if ($property.Name -in $arrayKeys) {
        if ($property.Value -isnot [System.Array]) {
            throw "'$($property.Name)' must be a JSON array."
        }
        foreach ($item in $property.Value) {
            if ($item -isnot [string] -or [string]::IsNullOrWhiteSpace($item)) {
                throw "'$($property.Name)' must contain only non-empty strings."
            }
        }
    }
    if ($property.Name -in $errorActionKeys -and $property.Value -notin @('Continue', 'Stop')) {
        throw "'$($property.Name)' must be 'Continue' or 'Stop'."
    }
}

$profile.PSObject.Properties.Name
```

Before completing the request, confirm that:

* The result is one flat object with at least one explicit setting.
* Every property starts with `Invoke-WinPEStartup:` and appears in the configuration reference.
* Switch-like settings are booleans.
* Module and command settings are arrays containing only non-empty strings.
* Error-action values are `Continue` or `Stop`.
* Command order matches the requested execution order.
* The resulting values match the user's requested behavior.
* Validation did not execute any profile command or URL.

{% hint style="warning" %}
The runtime loader may tolerate JSON comments, but standard JSON validation does not. Do not add comments to a profile.
{% endhint %}

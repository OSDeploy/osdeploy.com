---
description: Create a WinpeStartup profile that runs OSDCloud automatically and restarts after a successful deployment.
---

# OSDeploy Boot: Add a New WinpeStartup Profile

Create a custom WinpeStartup profile to display device information, start OSDCloud automatically, and restart the device after the deployment command completes successfully.

{% hint style="warning" %}
This profile starts deployment without leaving an interactive PowerShell window open and restarts immediately after `Deploy-OSDCloud` returns successfully. Test the profile before using it in production.
{% endhint %}

## Create the Profile

Create the shared profile directory and write `OSDCloud Restart.json`:

```powershell
$ProfileDirectory = Join-Path $env:ProgramData `
    'OSDeployCore\boot-assets\winpestartup-profiles'
$ProfilePath = Join-Path $ProfileDirectory 'OSDCloud Restart.json'

New-Item -Path $ProfileDirectory -ItemType Directory -Force | Out-Null

@'
{
  "env": {
    "WINPESTARTUP_AUTHOR": "OSDeploy",
    "WINPESTARTUP_PROFILE": "OSDCloud Restart"
  },
  "Invoke-WinpeStartup:InvokeMainCommand": [
    "Show-OSDCloudDeviceInfo",
    "Deploy-OSDCloud"
  ],
  "Invoke-WinpeStartup:InvokeMainCommandEA": "Stop",
  "Invoke-WinpeStartup:InvokeShutdownCommand": [
    "Restart-Computer -Force"
  ],
  "Invoke-WinpeStartup:InvokeShutdownCommandEA": "Stop"
}
'@ | Set-Content -LiteralPath $ProfilePath -Encoding utf8
```

`InvokeMainCommandEA` is set to `Stop` so a failed main command stops startup instead of continuing to the restart phase. The profile omits `InvokeMainCommandNoExit`; setting it to `true` would keep the child PowerShell session open and delay the restart until that window closes.

## Validate the JSON

Parse the file without executing any commands stored in it:

```powershell
$Profile = Get-Content -LiteralPath $ProfilePath -Raw | ConvertFrom-Json
$Profile |
    Format-List
```

Confirm that the output contains one `env` object, the two main commands, and the restart command. Do not define both `env` and `Environment` in the same profile.

## Add the WinpeStartup Profile to a Build

Start an interactive build:

```powershell
Build-OSDeployBoot
```

Select `OSDCloud Restart.json` when the WinpeStartup profile picker appears, then complete the remaining prompts. OSDeploy copies the selected JSON into `WinpeStartup\profiles` in the mounted image. Profile choices are sorted by name. Select only this WinpeStartup profile when startup must be automatic; including multiple WinpeStartup profiles causes a numbered selection prompt in WinPE.

## Test the Startup Flow

Boot the generated ISO in a test virtual machine before using physical hardware. Confirm this sequence:

1. WinPE starts and establishes the required network connection.
2. OSDCloud device information is displayed.
3. `Deploy-OSDCloud` starts.
4. A successful return from the deployment command triggers `Restart-Computer -Force`.
5. A terminating deployment error prevents the shutdown phase from running.

See the [WinpeStartup Profile Agent](../agents-skills/winpestartup-profile-agent.md) for every supported property and validation rule.

---
description: Add a custom PowerShell script that runs while Build-OSDeployBoot services the mounted WinPE image.
---

# OSDeploy Boot: Customize WinPE with a PowerShell Script

Place a PowerShell script in the shared `boot-winpescript` library when it must customize the mounted WinPE image during `Build-OSDeployBoot`.

{% hint style="info" %}
A WinPE build script runs on the OSDeploy workstation in the current PowerShell session. It does not become a startup script unless the script explicitly copies content into the mounted image.
{% endhint %}

## Create the Script

Create the shared script directory and a script that writes a marker file into WinPE:

```powershell
$ScriptDirectory = Join-Path $env:ProgramData 'OSDeployCore\boot-assets\boot-winpescript'
$ScriptPath = Join-Path $ScriptDirectory 'Add-ContosoMarker.ps1'

New-Item -Path $ScriptDirectory -ItemType Directory -Force | Out-Null

@'
$MountPath = $global:BuildMedia.MountPath

if (-not $MountPath -or -not (Test-Path -LiteralPath $MountPath -PathType Container)) {
    throw "The OSDeploy WinPE mount path is not available."
}

$Destination = Join-Path $MountPath 'Windows\System32\Contoso'
New-Item -Path $Destination -ItemType Directory -Force | Out-Null
Set-Content -LiteralPath (Join-Path $Destination 'Build.txt') `
    -Value "Built $([datetime]::Now.ToString('s'))" `
    -Encoding utf8
'@ | Set-Content -LiteralPath $ScriptPath -Encoding utf8
```

Use `$global:BuildMedia.MountPath` for files that belong inside WinPE. Throw when required input or an operation fails so the build log identifies the problem.

## Select the Script

Start an interactive build and select `Add-ContosoMarker.ps1` when the WinPE script picker appears:

```powershell
Build-OSDeployBoot
```

## Build and Verify

Complete the remaining prompts to build the media. The script runs near the end of mounted-image servicing, before WinPEStartup content and drivers are added. Script success-stream output is passed through by the build step; a script failure writes a non-terminating error and later build steps can continue.

Boot the resulting media and confirm this file exists:

```text
X:\Windows\System32\Contoso\Build.txt
```

Use `boot-mediascript` instead when a script must modify the completed media after the WIM is dismounted. See [OSDeployCore Boot-Assets](../reference/osdeploycore-boot-assets.md) for the two script lifecycles.

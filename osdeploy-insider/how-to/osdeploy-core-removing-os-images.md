---
description: Review and remove imported Windows OS source images from the OSDeploy Core cache.
---

# OSDeploy Core: Removing OS Images

`Update-OSDeployCoreRE` imports Windows Enterprise ESD content into versioned Windows OS source directories. Remove obsolete imports to recover space under:

```text
C:\ProgramData\OSDeployCore\cache\windows-os
```

{% hint style="warning" %}
The `windows-os` folder contains imported source content, not disposable download metadata. Deleting a source requires OSDeploy to import it again before workflows that depend on its setup media, boot files, or operating system image can use it.
{% endhint %}

## Understand the OS Source Directory

Each child directory identifies the build, revision, architecture, edition, and language:

```text
26300.9457-amd64-enterprise-en-us
26300.9457-arm64-enterprise-en-us
```

A source can contain `.core`, `.temp`, `.wim`, `WinOS-Media`, and `properties.json`. Its matching WinRE source uses the same directory name under `cache\windows-re`.

Deleting only the `windows-os` copy does not delete the paired WinRE source or completed boot media. Keep the matching pair when it is still required, and consider removing both only when the complete imported version is obsolete.

## Review Imported OS Images

Close active import and build operations, then open an elevated PowerShell 7 session.

List source metadata and approximate size:

```powershell
$WindowsOSRoot = Join-Path $env:ProgramData 'OSDeployCore\cache\windows-os'

$WindowsOSImages = Get-ChildItem -LiteralPath $WindowsOSRoot -Directory -ErrorAction SilentlyContinue |
    ForEach-Object {
        $PropertiesPath = Join-Path $_.FullName 'properties.json'
        $Properties = if (Test-Path -LiteralPath $PropertiesPath) {
            Get-Content -LiteralPath $PropertiesPath -Raw | ConvertFrom-Json
        }
        $Size = Get-ChildItem -LiteralPath $_.FullName -File -Recurse -ErrorAction SilentlyContinue |
            Measure-Object Length -Sum

        [pscustomobject]@{
            Name = $_.Name
            Version = $Properties.Version
            Architecture = $Properties.Architecture
            ModifiedTime = $_.LastWriteTime
            SizeGB = [math]::Round($Size.Sum / 1GB, 2)
            Path = $_.FullName
        }
    } |
    Sort-Object Version -Descending

$WindowsOSImages | Format-Table Name, Version, Architecture, ModifiedTime, SizeGB -AutoSize
```

Keep a source when it is the only imported version for a required architecture or when its paired WinRE image is still your tested boot source.

## Select OS Images to Remove

```powershell
$OldWindowsOSImages = $WindowsOSImages |
    Out-GridView -Title 'Select imported Windows OS images to remove' -PassThru

$OldWindowsOSImages |
    Format-Table Name, Version, Architecture, SizeGB, Path -AutoSize
```

## Preview and Remove

Preview the deletion:

```powershell
$OldWindowsOSImages.Path | Remove-Item -Recurse -Force -WhatIf
```

Confirm that every selected path is a direct child of `C:\ProgramData\OSDeployCore\cache\windows-os`.

Remove the selected sources:

```powershell
$OldWindowsOSImages.Path | Remove-Item -Recurse -Force -Confirm
```

Do not remove the `windows-os` root. OSDeploy recreates it during path initialization, but retaining the root makes the intended scope clear.

## Restore an OS Source

Confirm that the matching Enterprise ESD remains available under the OSDeploy Core OSDCloud cache, then run:

```powershell
Update-OSDeployCoreRE
```

Use `-Architecture` to restore only one architecture:

```powershell
Update-OSDeployCoreRE -Architecture 'amd64'
```

If the required ESD is absent, run `Update-OSDeployCoreESD` first. The import recreates the Windows OS directory and ensures that a matching WinRE directory is available.

See [OSDeployCore Cache](../reference/osdeploycore-cache.md) for the cache layout and [Update-OSDeployCoreRE](../../guide/cmdlets/update-osdeploycorere.md) for import requirements and behavior.

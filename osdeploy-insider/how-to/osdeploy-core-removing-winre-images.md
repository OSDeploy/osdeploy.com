---
description: Review and remove imported Windows Recovery Environment images from the OSDeploy Core cache.
---

# OSDeploy Core: Removing WinRE Images

`Update-OSDeployCoreRE` creates a Windows Recovery Environment source for each imported Windows OS source. Remove obsolete WinRE imports to recover space or remove old source choices from `Build-OSDeployBoot`:

```text
C:\ProgramData\OSDeployCore\cache\windows-re
```

{% hint style="warning" %}
`Build-OSDeployBoot` discovers WinRE sources from this directory. Keep at least one tested source for every architecture that requires WinRE-based media. When no compatible WinRE source is available, the build can fall back to the Windows ADK WinPE image.
{% endhint %}

## Understand the WinRE Source Directory

Each source normally contains:

```text
<source-name>\
|-- .core\
|-- .temp\
|-- .wim\
|   `-- winre.wim
`-- properties.json
```

The source name matches the paired directory under `cache\windows-os`. OSDeploy reads `properties.json` and `.wim\winre.wim` to populate WinRE source selection.

Deleting a WinRE source does not delete its paired Windows OS source, cached Enterprise ESD, or completed boot media.

## Review Imported WinRE Images

Close active import and build operations, then open an elevated PowerShell 7 session.

List valid WinRE sources and approximate size:

```powershell
$WindowsRERoot = Join-Path $env:ProgramData 'OSDeployCore\cache\windows-re'

$WindowsREImages = Get-ChildItem -LiteralPath $WindowsRERoot -Directory -ErrorAction SilentlyContinue |
    ForEach-Object {
        $PropertiesPath = Join-Path $_.FullName 'properties.json'
        $Properties = if (Test-Path -LiteralPath $PropertiesPath) {
            Get-Content -LiteralPath $PropertiesPath -Raw | ConvertFrom-Json
        }
        $ImagePath = Join-Path $_.FullName '.wim\winre.wim'
        $Size = Get-ChildItem -LiteralPath $_.FullName -File -Recurse -ErrorAction SilentlyContinue |
            Measure-Object Length -Sum

        [pscustomobject]@{
            Name = $_.Name
            Version = $Properties.Version
            Architecture = $Properties.Architecture
            HasWinRE = Test-Path -LiteralPath $ImagePath -PathType Leaf
            ModifiedTime = $_.LastWriteTime
            SizeGB = [math]::Round($Size.Sum / 1GB, 2)
            Path = $_.FullName
        }
    } |
    Sort-Object Version -Descending

$WindowsREImages |
    Format-Table Name, Version, Architecture, HasWinRE, ModifiedTime, SizeGB -AutoSize
```

Entries with `HasWinRE` set to `False` are incomplete and are not usable build sources.

## Select WinRE Images to Remove

```powershell
$OldWindowsREImages = $WindowsREImages |
    Out-GridView -Title 'Select imported WinRE images to remove' -PassThru

$OldWindowsREImages |
    Format-Table Name, Version, Architecture, HasWinRE, SizeGB, Path -AutoSize
```

Keep the newest known-working source for each required architecture.

## Preview and Remove

Preview the deletion:

```powershell
$OldWindowsREImages.Path | Remove-Item -Recurse -Force -WhatIf
```

Confirm that every selected path is a direct child of `C:\ProgramData\OSDeployCore\cache\windows-re`.

Remove the selected sources:

```powershell
$OldWindowsREImages.Path | Remove-Item -Recurse -Force -Confirm
```

Do not remove the `windows-re` root.

## Verify Build Source Selection

Run an interactive preview of the build source choices:

```powershell
Build-OSDeployBoot -WhatIf
```

Select a retained WinRE source and review the displayed build configuration. `-WhatIf` prevents the build-directory operation, but initialization and selection can still update local state.

## Restore a WinRE Source

Recreate a removed source from the verified Enterprise ESD:

```powershell
Update-OSDeployCoreRE
```

Restore one architecture when required:

```powershell
Update-OSDeployCoreRE -Architecture 'arm64'
```

If the ESD is absent, run `Update-OSDeployCoreESD` first. The import ensures that matching Windows OS and WinRE source directories exist.

See [OSDeployCore Cache](../reference/osdeploycore-cache.md) for the paired source layout and [Convert ESD to Windows RE](../reference/convert-esd-to-windows-re.md) for the extraction workflow.

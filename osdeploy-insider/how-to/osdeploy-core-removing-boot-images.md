---
description: Review and remove completed OSDeploy Boot image builds that are no longer required.
---

# OSDeploy Core: Removing Boot Images

`Build-OSDeployBoot` creates a new output directory for every build. Existing directories are not replaced; duplicate names receive a numeric suffix such as `-001`. Remove obsolete builds to recover space under:

```text
C:\ProgramData\OSDeployCore\boot
```

{% hint style="warning" %}
Do not remove a build that is being copied to USB, used by a virtual machine, or updated by `Update-OSDeployBootISO`. Keep at least one tested boot image until its replacement has been validated.
{% endhint %}

## Understand the Build Directory

A completed build can contain:

```text
<build-name>\
|-- .core\
|-- .temp\
|   `-- logs\
|-- bootmedia\
|   `-- sources\
|       `-- boot.wim
|-- bootmedia.iso
|-- bootmedia_ca2023\
`-- bootmedia_ca2023.iso
```

The `bootmedia_ca2023` content is present only when the selected WinRE source contains compatible updated Secure Boot files.

Deleting the complete build directory removes its WIM, ISO, logs, and generated media trees. It does not remove imported Windows OS or WinRE source images, shared Boot-Assets content, or cached applications.

## Review Boot Images

Close active OSDeploy builds and open an elevated PowerShell 7 session.

List every build with its ISO files and approximate size:

```powershell
$BootRoot = Join-Path $env:ProgramData 'OSDeployCore\boot'

$BootImages = Get-ChildItem -LiteralPath $BootRoot -Directory -ErrorAction SilentlyContinue |
    ForEach-Object {
        $Files = Get-ChildItem -LiteralPath $_.FullName -File -Recurse -ErrorAction SilentlyContinue
        [pscustomobject]@{
            Name = $_.Name
            ModifiedTime = $_.LastWriteTime
            SizeGB = [math]::Round(($Files | Measure-Object Length -Sum).Sum / 1GB, 2)
            ISO = (@($Files | Where-Object Extension -eq '.iso').Name -join ', ')
            Path = $_.FullName
        }
    } |
    Sort-Object ModifiedTime -Descending

$BootImages | Format-Table Name, ModifiedTime, SizeGB, ISO -AutoSize
```

The newest ISO is selected automatically by `New-OSDeployHyperVM` when `-ISO` is omitted. Confirm that an older build is not still required before removing it.

## Select Builds to Remove

Use `Out-GridView` to select obsolete build directories:

```powershell
$OldBootImages = $BootImages |
    Out-GridView -Title 'Select OSDeploy Boot images to remove' -PassThru

$OldBootImages | Format-Table Name, ModifiedTime, SizeGB, Path -AutoSize
```

If the selection is incorrect, run the selection command again.

## Preview and Remove

Preview the deletion:

```powershell
$OldBootImages.Path | Remove-Item -Recurse -Force -WhatIf
```

Confirm that every path is a direct child of `C:\ProgramData\OSDeployCore\boot`.

Remove the selected builds with a confirmation for each directory:

```powershell
$OldBootImages.Path | Remove-Item -Recurse -Force -Confirm
```

Do not remove the `boot` root. OSDeploy uses it for future build output.

## Verify and Rebuild

List the remaining content:

```powershell
Get-ChildItem -LiteralPath $BootRoot -Directory |
    Sort-Object LastWriteTime -Descending |
    Select-Object Name, LastWriteTime, FullName
```

Create replacement media when required:

```powershell
Build-OSDeployBoot
```

OSDeploy creates a new build directory; it does not restore the deleted directory name or its previous selections automatically.

See [Build-OSDeployBoot](../../guide/cmdlets/build-osdeployboot.md) for build behavior and [Update-OSDeployBootISO](../../guide/cmdlets/update-osdeploybootiso.md) for rebuilding ISO files from a retained media tree.

# Update-OSDeployISO

Rebuilds the bootable ISO files for a selected OSDeployCore BootImage build.

| Property | Value                                                                              |
|----------|------------------------------------------------------------------------------------|
| Module   | OSDeploy                                                                           |
| Platform | Windows 10 or later (PowerShell Core edition)                                      |
| Requires | Run as Administrator, Windows ADK installed, a completed BootImage build            |

## Description

Regenerates the ISO files (`bootmedia.iso` and optionally `bootmedia_ca2023.iso`) for a selected completed BootImage from `%ProgramData%\OSDeployCore\boot-media`. The existing `bootmedia` directory content is used as-is — no DISM mount or WIM modification is performed.

The function:

- Prompts for selection of a completed BootImage build
- Verifies the Windows ADK is installed
- Uses the installed ADK's `oscdimg.exe` to rebuild the ISO(s)
- Places the resulting ISOs in the BootImage's root directory

Use this function when you need to regenerate an ISO from existing bootmedia files without rebuilding the entire WinPE image. Use `Build-OSDeployBootMedia` to perform a full rebuild.

## Syntax

```powershell
Update-OSDeployISO
```

## Parameters

This function has no parameters. The BootImage selection is made via an interactive Out-GridView picker.

## Examples

```powershell
# Prompt for a BootImage build selection and rebuild its ISO files
Update-OSDeployISO
```

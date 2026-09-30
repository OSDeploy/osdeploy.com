---
description: Add a custom JPG background to an OSDeploy WinPE boot image.
---

# OSDeploy Boot: Add Custom Wallpaper to WinPE

Store one or more JPG images in the shared OSDeploy Boot-Assets wallpaper folder, then select the image during `Build-OSDeployBoot`. OSDeploy replaces `Windows\System32\winpe.jpg` in the mounted WinPE image with the selected file.

## Requirements

Prepare the OSDeploy workstation and source content before starting:

* Run PowerShell 7.6 or later as an administrator.
* Install the current OSDeploy module.
* Use a `.jpg` file. The wallpaper selector does not discover other image formats.
* Keep the JPG directly in the shared wallpaper folder. Subdirectories are not searched.

{% hint style="info" %}
Test the image at the display resolutions used by the target devices. OSDeploy copies the JPG without resizing or converting it.
{% endhint %}

## Add the Wallpaper

Set the source image and create the shared wallpaper directory:

```powershell
$Source = 'C:\Branding\Contoso-WinPE.jpg'
$WallpaperDirectory = Join-Path $env:ProgramData `
    'OSDeployCore\boot-assets\boot-wallpaper'

if (-not (Test-Path -LiteralPath $Source -PathType Leaf)) {
    throw "Wallpaper not found: $Source"
}

New-Item -Path $WallpaperDirectory -ItemType Directory -Force | Out-Null
Copy-Item -LiteralPath $Source -Destination $WallpaperDirectory -Force
```

List the available wallpapers:

```powershell
Get-ChildItem -LiteralPath $WallpaperDirectory -Filter '*.jpg' -File |
    Sort-Object Name |
    Select-Object Name, Length, FullName
```

## Build the Boot Image

Start an interactive build:

```powershell
Build-OSDeployBoot
```

Wallpaper selection depends on the number of JPG files in the shared folder:

| JPG files | Build behavior |
| --- | --- |
| None | Use the default wallpaper bundled with the OSDeploy module. |
| One | Select the JPG automatically. |
| More than one | Display an `Out-GridView` picker for one JPG. |

When multiple images are available, select the required image and choose **OK**. Cancelling the picker causes the build to use the bundled default wallpaper.

During image servicing, OSDeploy takes ownership of the existing `Windows\System32\winpe.jpg`, grants the local Administrators group access, removes the existing file, and copies the selected JPG into its place.

## Verify the Result

Complete the build and boot the generated ISO in a test virtual machine or test device. Confirm that the custom image appears as the WinPE desktop background.

If the default image appears instead:

1. Confirm that the source file uses the `.jpg` extension.
2. Confirm that the file is directly below `C:\ProgramData\OSDeployCore\boot-assets\boot-wallpaper`.
3. Run `Build-OSDeployBoot` again and select the intended image when more than one JPG exists.
4. Review the build output for the `Copying WinPE wallpaper` message or a missing-wallpaper warning.

See [OSDeployCore Boot-Assets](../reference/osdeploycore-boot-assets.md) for the complete shared-content directory layout and wallpaper precedence.

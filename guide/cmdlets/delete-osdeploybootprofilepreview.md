---
description: Delete an OSDeploy Boot profile and its profile-local content.
---

# Delete-OSDeployBootProfilePreview

`Delete-OSDeployBootProfilePreview` deletes an existing profile directory and every profile-local file beneath it. It does not remove generated boot media, shared Boot-Assets content, or cached content.

## Requirements

Run the command from a PowerShell session with permission to remove the selected profile directory. The exact canonical profile name must identify a directory containing `osdeployboot.json` under `%ProgramData%\OSDeployCore\boot-assets\osdeployboot-profiles`.

The command does not use the shared OSDeploy license gate.

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | None | Required. Specify the exact canonical profile name. Values tab-complete from readable profile directories. |
| `-Force` | `Switch` | Not enabled | Required to enable deletion. Without it, the command warns and makes no change. |
| `-WhatIf` | Common parameter | Not enabled | With `-Force`, report the recursive profile-directory deletion without removing content. |
| `-Confirm` | Common parameter | Not enabled | With `-Force`, confirm recursive deletion. |

## Examples

### Check the force requirement

```powershell
Delete-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64'
```

The command locates the profile, warns that `-Force` is required, and leaves it unchanged.

### Delete a profile

```powershell
Delete-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64' -Force
```

### Preview recursive deletion

```powershell
Delete-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64' -Force -WhatIf
```

## Deletion Boundary

Profile lookup occurs before the `-Force` check and before `ShouldProcess`. When `-Force` is present and the operation is approved, the command removes the entire profile directory recursively.

{% hint style="danger" %}
Profile deletion cannot be undone by this command. Review profile-local scripts, drivers, startup content, and wallpaper before approving deletion.
{% endhint %}

Generated content under `%ProgramData%\OSDeployCore\boot`, shared content elsewhere under `boot-assets`, and content under `cache` are not removed.

## Output

The command writes no object to the pipeline.

Create another profile with [New-OSDeployBootProfilePreview](new-osdeploybootprofilepreview.md), or see the compact [Delete-OSDeployBootProfilePreview command reference](../../command-reference/osdeploy/delete-osdeploybootprofilepreview.md).

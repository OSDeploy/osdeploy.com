# Delete-OSDeployBootProfilePreview

Deletes an OSDeploy Boot profile directory and all profile-local content.

## Syntax

```powershell
Delete-OSDeployBootProfilePreview [-ProfileName] <String> [-Force] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-ProfileName` | `String` | Yes | Exact canonical name of an existing profile. |
| `-Force` | `Switch` | No | Enables deletion. Without it, the command warns and makes no change. |
| `-WhatIf` | `Switch` | No | Preview recursive profile-directory deletion. |
| `-Confirm` | `Switch` | No | Confirm deletion. |

## Examples

```powershell
Delete-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64'
```

```powershell
Delete-OSDeployBootProfilePreview -ProfileName 'Contoso-amd64' -Force
```

Generated boot media, shared repository content, and cached content are not removed. Deletion is recursive and cannot be undone by this command. The command returns no pipeline object.

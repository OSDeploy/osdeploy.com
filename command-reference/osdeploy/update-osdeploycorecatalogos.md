# Update-OSDeployCoreCatalogOS

Acquires, validates, publishes, and caches the current Windows 11 operating system catalog.

## Syntax

```powershell
Update-OSDeployCoreCatalogOS [-MinimumItemCount <Int32>] [-WhatIf] [-Confirm]
```

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `-MinimumItemCount` | `Int32` | No | Minimum ESD record count. Defaults to `50`; accepts `1` through `100000`. |
| `-WhatIf` | `Switch` | No | Acquires and validates, but previews publication and cache synchronization. |
| `-Confirm` | `Switch` | No | Prompts before publication and cache synchronization. |

## Output

Returns a catalog status `System.Management.Automation.PSCustomObject` after successful validation. Failures produce warnings and no object.

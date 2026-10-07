---
description: Acquire, validate, publish, and cache the current Windows 11 operating system catalog.
---

# Update-OSDeployCoreCatalogOS

`Update-OSDeployCoreCatalogOS` downloads the current Windows 11 products catalog from Microsoft, validates it, publishes it in the OSDeploy module, and synchronizes the OSDeploy Core catalog cache.

## Requirements

Run the command from an elevated PowerShell 7.6 or later session on Windows 11 25H2 build 26200 or Windows 11 26H2 build 26300. Install ADK `10.1.26100.2454` or `10.1.26100.9457`. Internet access, `expand.exe`, writable temporary storage, and write access to the module and Core cache are also required.

The host build and installed ADK limit the catalog release. ADK `10.1.26100.2454` selects Windows 11 25H2; ADK `10.1.26100.9457` selects Windows 11 26H2 only when the host is also 26H2. Unsupported Windows or ADK versions stop the command before its Microsoft request.

## Parameters

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |
| `-MinimumItemCount` | `Int32` | `50` | Accepts `1` through `100000`. Rejects a catalog with fewer ESD records. |
| `-WhatIf` | Common parameter | Not enabled | Still acquires, extracts, and validates the catalog; previews only publication and cache synchronization. |
| `-Confirm` | Common parameter | Not enabled | Prompts before publication and cache synchronization. |

## Examples

### Update the catalog

```powershell
Update-OSDeployCoreCatalogOS
```

### Validate without publishing

```powershell
Update-OSDeployCoreCatalogOS -WhatIf
```

## Validation and Output

The command verifies the Microsoft CAB size and SHA256 digest, validates required catalog properties, and requires en-US Enterprise x64 and ARM64 records. It names the catalog from the embedded build and release identity, such as `26300.9457.260913-1737.xml`.

After successful validation, it returns a `System.Management.Automation.PSCustomObject` with `Build`, `ModuleCatalogPath`, `CacheCatalogPath`, `Published`, `Cached`, `ItemCount`, and `Sha256`. Failures produce warnings and no object.

Continue with [Update-OSDeployCoreESD](update-osdeploycoreesd.md), or run [Update-OSDeployCore](update-osdeploycore.md) for the complete refresh.

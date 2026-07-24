# Get-OSDCloudAzureResources

Discover OSDCloud Azure Storage resources.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Uses the current Azure context to enumerate storage accounts tagged OSDCloud, caches the
matching containers and blob collections in global variables, and writes discovered resource
snapshots to the WinPE log folder when available.

## Syntax

```powershell
Get-OSDCloudAzureResources [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

### Example
```powershell
Get-OSDCloudAzureResources
Scans Azure storage for tagged OSDCloud resources.
```

## Related

* [https://github.com/OSDeploy/OSD/tree/master/docs](https://github.com/OSDeploy/OSD/tree/master/docs)

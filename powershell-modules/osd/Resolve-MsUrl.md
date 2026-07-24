# Resolve-MsUrl

Resolves a short Microsoft aka.ms or fwlink URL.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Resolves a short Microsoft aka.ms or fwlink URL.

## Syntax

```powershell
Resolve-MsUrl [-Uri] <Uri> [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Uri` | `Uri` | True | Uri to resolve. |

## Examples

### Example
```powershell
Resolve-MsUrl -Uri 'https://aka.ms/windows'
```

## Related

* [https://osd.osdeploy.com](https://osd.osdeploy.com)

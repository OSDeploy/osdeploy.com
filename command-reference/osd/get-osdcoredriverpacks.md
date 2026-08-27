# Get-OSDCoreDriverPacks

Retrieves driver pack information for the specified manufacturer and operating system architecture.

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

Gets driver pack catalogs based on the device manufacturer and OS architecture.
For AMD64 architecture,
manufacturer-specific catalogs are loaded.
For ARM64 and other architectures, the default catalog is returned.
Supports Dell, HP, Lenovo, Microsoft (Surface), and generic devices.

## Syntax

```powershell
Get-OSDCoreDriverPacks [[-GenericDriverPackJson] <String>] [[-OSDManufacturer] <String>]
 [[-ProcessorArchitecture] <String>] [-ProgressAction <ActionPreference>] [<CommonParameters>]
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-GenericDriverPackJson` | `String` | False | No additional description provided. |
| `-OSDManufacturer` | `String` | False | The device manufacturer name. Defaults to the value from $global:OSDCoreDevice.OSDManufacturer. Supported values: Dell, HP, Lenovo, Microsoft, or any other value will use the Default catalog. |
| `-ProcessorArchitecture` | `String` | False | The operating system architecture. Defaults to the value from $global:OSDCoreDevice.ProcessorArchitecture. Typically 'amd64' or 'arm64'. |

## Examples

### Example
```powershell
Get-OSDCoreDriverPacks
Returns driver packs for the current device's manufacturer and architecture.
```

### Example
```powershell
Get-OSDCoreDriverPacks -OSDManufacturer 'Dell' -ProcessorArchitecture 'amd64'
Returns driver packs for Dell devices with AMD64 architecture.
```

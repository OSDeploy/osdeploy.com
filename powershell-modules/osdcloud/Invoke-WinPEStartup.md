# Invoke-WinPEStartup

Runs the WinPE startup workflow for OSDCloud.

| Property | Value                                       |
|----------|---------------------------------------------|
| Module   | OSDCloud                                    |
| Platform | WinPE only (`SystemDrive == X:`)            |
| Requires | OSDCloud module, WinPE environment          |

{% hint style="info" %}
This function only runs in WinPE. When called from a full Windows OS, it writes a warning and exits without performing any actions.
{% endhint %}

## Description

Executes the OSDCloud WinPE startup sequence from a single entry point. Optionally loads defaults from module JSON, discovers and applies a startup profile, then runs startup steps in order: environment setup, drivers, files, hardware checks, connectivity, module updates, script execution, and optional URL or command invocations.

HTTP and HTTPS entries in `-InvokeStartupCommand`, `-InvokeMainCommand`, and `-InvokeShutdownCommand` are automatically wrapped as `Invoke-RestMethod -Uri <url> | Invoke-Expression`.

## Syntax

```powershell
Invoke-WinPEStartup
    [-SkipOnScreenKeyboard]
    [-ShowPnpDevices]
    [-ShowPnpErrors]
    [-SkipWiFi]
    [-SkipIPConfig]
    [-SkipUpdateOSDCloud]
    [-InstallModule <String[]>]
    [-InvokeStartupCommand <String[]>]
    [-InvokeStartupCommandNoExit]
    [-InvokeStartupCommandEA <String>]
    [-InvokeMainCommand <String[]>]
    [-InvokeMainCommandNoExit]
    [-InvokeMainCommandEA <String>]
    [-InvokeShutdownCommand <String[]>]
    [-InvokeShutdownCommandNoExit]
    [-InvokeShutdownCommandEA <String>]
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `-SkipOnScreenKeyboard` | `Switch` | — | Skips launching the on-screen keyboard check. |
| `-ShowPnpDevices` | `Switch` | — | Shows the PnP device hardware window (`Show-WinPEStartupDevices`). Hidden by default. |
| `-ShowPnpErrors` | `Switch` | — | Shows the PnP device error window (`Show-WinPEStartupDeviceErrors`). Hidden by default. |
| `-SkipWiFi` | `Switch` | — | Skips Wi-Fi startup and connection checks. |
| `-SkipIPConfig` | `Switch` | — | Skips displaying IP configuration details. |
| `-SkipUpdateOSDCloud` | `Switch` | — | Skips updating the OSDCloud module during startup. |
| `-InstallModule` | `String[]` | — | One or more additional PowerShell module names to install or update during startup. |
| `-InvokeStartupCommand` | `String[]` | — | PowerShell commands or URLs to execute during the startup phase. |
| `-InvokeStartupCommandNoExit` | `Switch` | — | Keeps the child PowerShell window open after startup commands complete. |
| `-InvokeStartupCommandEA` | `String` | `Continue` | Error action for the startup command child process. Valid values: `Continue`, `Stop`. |
| `-InvokeMainCommand` | `String[]` | — | PowerShell commands or URLs to execute during the main phase. |
| `-InvokeMainCommandNoExit` | `Switch` | — | Keeps the child PowerShell window open after main commands complete. |
| `-InvokeMainCommandEA` | `String` | `Continue` | Error action for the main command child process. Valid values: `Continue`, `Stop`. |
| `-InvokeShutdownCommand` | `String[]` | — | PowerShell commands or URLs to execute during the shutdown phase. |
| `-InvokeShutdownCommandNoExit` | `Switch` | — | Keeps the child PowerShell window open after shutdown commands complete. |
| `-InvokeShutdownCommandEA` | `String` | `Continue` | Error action for the shutdown command child process. Valid values: `Continue`, `Stop`. |

## Examples

```powershell
# Run the default WinPE startup sequence
Invoke-WinPEStartup
```

```powershell
# Run with detailed verbose output
Invoke-WinPEStartup -Verbose
```

```powershell
# Skip Wi-Fi and IP configuration steps
Invoke-WinPEStartup -SkipWiFi -SkipIPConfig
```

```powershell
# Run a PSCloudScript URL during the main phase
Invoke-WinPEStartup -InvokeMainCommand 'https://raw.githubusercontent.com/OSDeploy/OSDCloud/main/Deploy-OSDCloud.ps1'
```

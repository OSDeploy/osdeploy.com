# Update-WinPEStartupModule

Installs or updates a PowerShell module from the PowerShell Gallery in WinPE.

| Property | Value                                                   |
|----------|---------------------------------------------------------|
| Module   | OSDCloud                                                |
| Platform | WinPE only                                              |
| Requires | OSDCloud module, WinPE environment, internet access     |

{% hint style="info" %}
This function uses `AllUsers` scope with `-Force` and `-SkipPublisherCheck`. A 10-second countdown is displayed before installation begins.
{% endhint %}

## Description

Installs the latest version of a specified PowerShell module using `Install-Module` with `AllUsers` scope, then imports the module with `-Force`. A 10-second countdown is displayed before installation begins to allow the operator to observe the update.

## Syntax

```powershell
Update-WinPEStartupModule -Name <String>
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `-Name` | `String` | Yes | The PowerShell module name to install or update from the PowerShell Gallery. |

## Examples

```powershell
# Update the OSDCloud module and import it
Update-WinPEStartupModule -Name OSDCloud
```

```powershell
# Install or update the PSDiskPart module
Update-WinPEStartupModule -Name PSDiskPart
```

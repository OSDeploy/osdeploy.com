# Set-SetupCompleteOSDCloudUSB

This function copies SetupComplete Files to the Local OSDCloud SetupComplete Folder

| Property | Value |
|---|---|
| Module | OSD |
| Platform | WinPE (amd64 / arm64) |

## Description

This function checks for the presence of an OSDCLoud SetupComplete Folder on any drive other than 'C'.
Sorts the drives in Descending order and returns $true if the SetupComplete Folder with files inside is found.
Copies the SetupComplete Files to the Local OSDCloud SetupComplete Folder.
Then onfigures the System SetupComplete.ps1 File to run the Custom Scripts from the OSDCloud SetupComplete Folder.

## Syntax

```powershell
Set-SetupCompleteOSDCloudUSB
```

## Parameters

This function does not define function-specific parameters beyond common parameters.

## Examples

No examples provided in source documentation.

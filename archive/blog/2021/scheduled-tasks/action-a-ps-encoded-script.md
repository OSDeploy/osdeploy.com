# Action a PS Encoded Script

The proper way to Action a PowerShell script is to either convert it to a ONE LINER, or simply Encode the contents.  In this example, I want to create a Scheduled Task with the following results

* Export third party drivers to C:\Exported Drivers
* Log to C:\Windows\Logs\Drivers
* Run as SYSTEM for Standard Users

## Action Script

The following script is what would be run to export the drivers to C:\ExportedDrivers and to write a Transcript to C:\Windows\Logs\Drivers

```
$TaskName = 'Export-WindowsDriverOnline'
#======================================================================================
#   Logs
#======================================================================================
$TaskLogs = "$env:SystemRoot\Logs\Drivers"
if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
$TaskLogName = "$((Get-Date).ToString('yyyy-MM-dd-HHmmss'))-$TaskName.log"
Start-Transcript -Path (Join-Path $TaskLogs $TaskLogName)
#======================================================================================
#   Main
#======================================================================================
#if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
Export-WindowsDriver -Online -Destination $env:SystemDrive\ExportedDrivers
#======================================================================================
#   Complete
#======================================================================================
Stop-Transcript
```

## Encoding the Command

Its easy enough to put the script into a Variable `$TaskScript` and convert the Variable to `$EncodedCommand`

```
#======================================================================================
#   Script
$TaskScript = @'
$TaskName = 'Export-WindowsDriverOnline'
#======================================================================================
#   Logs
#======================================================================================
$TaskLogs = "$env:SystemRoot\Logs\Drivers"
if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
$TaskLogName = "$((Get-Date).ToString('yyyy-MM-dd-HHmmss'))-$TaskName.log"
Start-Transcript -Path (Join-Path $TaskLogs $TaskLogName)
#======================================================================================
#   Main
#======================================================================================
#if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
Export-WindowsDriver -Online -Destination $env:SystemDrive\ExportedDrivers
#======================================================================================
#   Complete
#======================================================================================
Stop-Transcript
'@
#======================================================================================
#   Encode the Script
$EncodedCommand = [System.Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes($TaskScript))
```

## Splat the Action

The following example is how to Action the `$EncodedCommand` in the Scheduled Task

```
$Action = @{
    Execute = 'powershell.exe'
    Argument = "-ExecutionPolicy ByPass -EncodedCommand $EncodedCommand"
}
```

## Full Script

Once I have that information, I can complete the script for creating my Scheduled Task

```
#======================================================================================
#   Task Properties
$TaskName = 'Export-WindowsDriverOnline'
$TaskPath = '\Corporate\Drivers'
$Description = @"
Exports third party drivers to $env:SystemDrive\ExportedDrivers
Transcripts are stored in $env:SystemRoot\Logs\Drivers  
Runs as SYSTEM and does not display any progress or results  
PowerShell Encoded Script  
Version 21.1.19
"@
#======================================================================================
#   Script
$TaskScript = @'
$TaskName = 'Export-WindowsDriverOnline'
#======================================================================================
#   Logs
#======================================================================================
$TaskLogs = "$env:SystemRoot\Logs\Drivers"
if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
$TaskLogName = "$((Get-Date).ToString('yyyy-MM-dd-HHmmss'))-$TaskName.log"
Start-Transcript -Path (Join-Path $TaskLogs $TaskLogName)
#======================================================================================
#   Main
#======================================================================================
#if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
Export-WindowsDriver -Online -Destination $env:SystemDrive\ExportedDrivers
#======================================================================================
#   Complete
#======================================================================================
Stop-Transcript
'@
#======================================================================================
#   Encode the Script
$EncodedCommand = [System.Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes($TaskScript))
#======================================================================================
#   Splat the Task
$Action = @{
    Execute = 'powershell.exe'
    Argument = "-ExecutionPolicy ByPass -EncodedCommand $EncodedCommand"
}
$Principal = @{
    UserId = 'SYSTEM'
    RunLevel = 'Highest'
}
$Settings = @{
    AllowStartIfOnBatteries = $true
    Compatibility = 'Win8'
    MultipleInstances = 'Parallel'
    ExecutionTimeLimit = (New-TimeSpan -Minutes 60)
}
$ScheduledTask = @{
    Action = New-ScheduledTaskAction @Action
    Principal = New-ScheduledTaskPrincipal @Principal
    Settings = New-ScheduledTaskSettingsSet @Settings
    Description = $Description
}
#======================================================================================
#   Build the Task
New-ScheduledTask @ScheduledTask | Register-ScheduledTask -TaskName $TaskName -TaskPath $TaskPath -Force
#======================================================================================
#   Apply Authenticated User Permissions
$Scheduler = New-Object -ComObject "Schedule.Service"
$Scheduler.Connect()
$GetTask = $Scheduler.GetFolder($TaskPath).GetTask($TaskName)
$GetSecurityDescriptor = $GetTask.GetSecurityDescriptor(0xF)
if ($GetSecurityDescriptor -notmatch 'A;;0x1200a9;;;AU') {
    $GetSecurityDescriptor = $GetSecurityDescriptor + '(A;;GRGX;;;AU)'
    $GetTask.SetSecurityDescriptor($GetSecurityDescriptor, 0)
}
```

## Results

The Task gets created properly with the Encoded script

![](<../../../../.gitbook/assets/image (171).png>)

![](<../../../../.gitbook/assets/image (247).png>)

Running the Task works flawlessly in exporting the drivers

![](<../../../../.gitbook/assets/image (298).png>)

And everything is logged in the PowerShell Transcript, including the Encoded Script

![](<../../../../.gitbook/assets/image (157).png>)

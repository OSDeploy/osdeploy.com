# Task Trigger

A Trigger adds automation to the Scheduled Task.  While I have a Scheduled Task that allows a Standard User to change their Execution Policy, the User may forget to change things back.  There are many types of Triggers that can be set, and I suggest reading about them from Microsoft

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/new-scheduledtasktrigger?view=win10-ps" %}

In my case, I want to set the Execution Policy back to Restricted as soon as the computer is rebooted, so I will add a Trigger to set things back to Restricted the next time the Computer is restarted.  Its easy enough to Splat the setting and add it to the Task using **`New-ScheduledTaskTrigger`**

```
$Trigger = @{
    AtStartup = $true
}
$ScheduledTask = @{
    Action = New-ScheduledTaskAction @Action
    Principal = New-ScheduledTaskPrincipal @Principal
    Settings = New-ScheduledTaskSettingsSet @Settings
    Trigger = New-ScheduledTaskTrigger @Trigger
    Description = $Description
}
```

## Full Script

Here is a copy of the full script.  I have modified the Description a bit and added a Version

```
#Requires -RunAsAdministrator

$TaskName = 'Set-ExecutionPolicy Restricted AtStartup'
$TaskPath = '\Corporate\PowerShell'
$Description = @"
Version 21.1.18  
Set-ExecutionPolicy Restricted -Force  
Runs as SYSTEM at system startup and does not display any progress or results
"@

$Action = @{
    Execute = 'powershell.exe'
    Argument = 'Set-ExecutionPolicy Restricted -Force'
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
$Trigger = @{
    AtStartup = $true
}
$ScheduledTask = @{
    Action = New-ScheduledTaskAction @Action
    Principal = New-ScheduledTaskPrincipal @Principal
    Settings = New-ScheduledTaskSettingsSet @Settings
    Trigger = New-ScheduledTaskTrigger @Trigger
    Description = $Description
}

New-ScheduledTask @ScheduledTask | Register-ScheduledTask -TaskName $TaskName -TaskPath $TaskPath -Force

$Scheduler = New-Object -ComObject "Schedule.Service"
$Scheduler.Connect()
$GetTask = $Scheduler.GetFolder($TaskPath).GetTask($TaskName)
$GetSecurityDescriptor = $GetTask.GetSecurityDescriptor(0xF)
if ($GetSecurityDescriptor -notmatch 'A;;0x1200a9;;;AU') {
    $GetSecurityDescriptor = $GetSecurityDescriptor + '(A;;GRGX;;;AU)'
    $GetTask.SetSecurityDescriptor($GetSecurityDescriptor, 0)
}
```

## Task Scheduler

Everything looks great.  Now when the computer is restarted, the Execution Policy will always be set to Restricted

![](<../../../../.gitbook/assets/image (302).png>)








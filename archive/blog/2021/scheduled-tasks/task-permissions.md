# Task Permissions

Every Task contains permissions, called a Security Descriptor which defines who has rights to the Scheduled Task

## Regedit

You can find the Security Descriptor of your task by looking in the Registry

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree
```

![](<../../../../.gitbook/assets/image (398).png>)

## PowerShell

You can get the Security Descriptor in PowerShell using the Task Scheduler API

{% embed url="https://docs.microsoft.com/en-us/windows/win32/api/_taskschd/" %}

```
$TaskScheduler = New-Object -ComObject Schedule.Service
$TaskScheduler.Connect()
$Task = $TaskScheduler.GetFolder('\PowerShell').GetTask('Set-ExecutionPolicy Bypass')
$SecurityDescriptor = $Task.GetSecurityDescriptor(0xF)
Write-Host "SecurityDescriptor:" -ForegroundColor Cyan
$SecurityDescriptor
```

![](<../../../../.gitbook/assets/image (291).png>)

If you want to know more about Security Descriptor Definition Language, feel free to study this link

{% embed url="https://docs.microsoft.com/en-us/windows/win32/secauthz/security-descriptor-definition-language" %}

To convert the SDDL String to an ACL, simply run this command

```
(ConvertFrom-SddlString -Sddl $SecurityDescriptor).DiscretionaryAcl
```

![](<../../../../.gitbook/assets/image (260).png>)

Which should confirm that a Standard User does not have rights to READ or EXECUTE the Scheduled Task

## Granting Access

Granting access to Authenticated Users for READ and EXECUTE is as simple as connecting to the Task Scheduler API and adding **`(A;;GRGX;;;AU)`** to the Security Descriptor using the following code

```
$Scheduler = New-Object -ComObject "Schedule.Service"
$Scheduler.Connect()
$GetTask = $Scheduler.GetFolder($TaskPath).GetTask($TaskName)
$GetSecurityDescriptor = $GetTask.GetSecurityDescriptor(0xF)
if ($GetSecurityDescriptor -notmatch 'A;;0x1200a9;;;AU') {
    $GetSecurityDescriptor = $GetSecurityDescriptor + '(A;;GRGX;;;AU)'
    $GetTask.SetSecurityDescriptor($GetSecurityDescriptor, 0)
}
```

## Full Script

Here is the full script to run

```
#Requires -RunAsAdministrator

$TaskName = 'Set-ExecutionPolicy Bypass'
$TaskPath = '\Corporate\PowerShell'
$Description = @"
Set-ExecutionPolicy Bypass -Force  
Runs as SYSTEM and does not display any progress or results
"@

$Action = @{
    Execute = 'powershell.exe'
    Argument = 'Set-ExecutionPolicy Bypass -Force'
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

## Results

After running the PowerShell script as an Administrator, I can now log in as a Standard User and see the Task in Task Scheduler.  The PowerShell window shows the current Execution Policy, an error showing that I don't have permissions to change the Execution Policy, and finally running the Scheduled Task and displaying the Execution Policy results after running the Task

![](<../../../../.gitbook/assets/image (224).png>)

## References

{% embed url="https://michlstechblog.info/blog/windows-run-task-scheduler-task-as-limited-user/" %}

{% embed url="https://superuser.com/questions/1475639/how-to-fix-broken-permissions-for-windows-scheduled-task" %}

---
description: PowerShell ScheduledTasks Module
---

# Scheduled Tasks

Its been a while since I have done a writeup, and after spending a few days working with PowerShell and Scheduled Tasks, its time to get some work done.  The goal of this writeup is to detail how to make a Scheduled Task using PowerShell and to run these Tasks as a Standard User with the SYSTEM account.  A good start is to link to Microsoft's official word on the ScheduledTasks Module

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/?view=win10-ps" %}

## A Simple Task

Microsoft has some basic examples on how to create a Scheduled Task, so here is the example they give.  I strongly recommend understanding how this is done before going forward to the next article

```
PS C:\> $A = New-ScheduledTaskAction -Execute "Taskmgr.exe"
PS C:\> $T = New-ScheduledTaskTrigger -AtLogon
PS C:\> $P = New-ScheduledTaskPrincipal "Contoso\Administrator"
PS C:\> $S = New-ScheduledTaskSettingsSet
PS C:\> $D = New-ScheduledTask -Action $A -Principal $P -Trigger $T -Settings $S
PS C:\> Register-ScheduledTask T1 -InputObject $D
```

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/new-scheduledtask?view=win10-ps" %}

## Adam The Automator

Probably the best written PowerShell and Scheduled Tasks article is from [June Castillote](https://twitter.com/junecastillote) and posted at the following link.  Give it a read, and give June a Twitter follow!

{% embed url="https://adamtheautomator.com/powershell-scheduled-task/" %}

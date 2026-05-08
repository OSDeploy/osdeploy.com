# Building a Task

Things can get very complicated when building a task as there are several parameters to define, so its much easier to use Splatting to define the Variables. I suggest you get to know what Splatting is before going any further

{% embed url="https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_splatting?view=powershell-5.1" %}

## Building a Scheduled Task

I'll start with some basic information I need about the Scheduled Task. Since this Scheduled Task will run Admin level, it will need to run with Admin level access

#### TASKNAME

This will be the Name of the Task you are creating in Task Scheduler

#### TASKPATH

While not necessary, it keeps Tasks from cluttering up the ROOT of Task Scheduler. If you are creating several Tasks for your Organization, I recommend using a common root, such as 'Corporate'

![](<../../../../.gitbook/assets/image (175).png>)

#### DESCRIPTION

Again, not necessary, but highly recommended so you know exactly what is happening in the Task. I recommend putting some Version information in the Description

### Task Properties

In this example, I am going to create a Scheduled Task that will set the PowerShell ExecutionPolicy to Bypass ... so here is how things look

```
#Requires -RunAsAdministrator
$TaskName = "Set-ExecutionPolicy Bypass"
$TaskPath = "\Corporate\PowerShell"
$Description = @"
Set-ExecutionPolicy Bypass -Force  
Runs as SYSTEM and does not display any progress or results
"@
```

### New-ScheduledTaskAction

The Action is what is going to run in the Scheduled Task, which will be PowerShell with a Command

```
$Action = @{
    Execute = 'powershell.exe'
    Argument = 'Set-ExecutionPolicy Bypass -Force'
}
```

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/new-scheduledtaskaction?view=win10-ps" %}

### New-ScheduledTaskPrincipal

The Principal is the account that will be used to run the Scheduled Task. For this Task I will use SYSTEM at the Highest RunLevel. The Scheduled Task will run whether someone is logged in or not

```
$Principal = @{
    UserId = 'SYSTEM'
    RunLevel = 'Highest'
}
```

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/new-scheduledtaskprincipal?view=win10-ps" %}

### New-ScheduledTaskSettingsSet

This function will define the settings. For this Scheduled Task I want to make sure it will run whether the computer has AC power or is on Battery, as well as limiting it to running for a maximum of 60 minutes. Win8 Compatibility is for Windows 10, and multiple instances are run in Parallel (at the same time) if it is currently running

```
$Settings = @{
    AllowStartIfOnBatteries = $true
    Compatibility = 'Win8'  
    MultipleInstances = 'Parallel'
    ExecutionTimeLimit = (New-TimeSpan -Minutes 60)
}
```

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/new-scheduledtasksettingsset?view=win10-ps" %}

### New-ScheduledTask

Once I have all of my settings defined, I can now splat the Scheduled Task variables

```
$ScheduledTask = @{
    Action = New-ScheduledTaskAction @Action
    Principal = New-ScheduledTaskPrincipal @Principal
    Settings = New-ScheduledTaskSettingsSet @Settings
    Description = $Description
}
```

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/new-scheduledtask?view=win10-ps" %}

### Register-ScheduledTask

Registering a Scheduled Task is what publishes it into Scheduled Tasks. Its important to use the `-Force` parameter as this will allow me to overwrite previous Tasks

```
New-ScheduledTask @ScheduledTask | Register-ScheduledTask -TaskName $TaskName -TaskPath $TaskPath -Force
```

{% embed url="https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/register-scheduledtask?view=win10-ps" %}

## The Complete Script

With a complete script, run in PowerShell (as Admin) to create the Task

```
#Requires -RunAsAdministrator

$TaskName = 'Set-ExecutionPolicy Bypass'
$TaskPath = '\Corporate\PowerShell'
$Description = @"
Version 21.1.19  
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
```

## The New Scheduled Task

The screenshots below are the Scheduled Task Properties. The highlighted areas were modified as a result of the values from the PowerShell script

![](<../../../../.gitbook/assets/image (180).png>)

![](<../../../../.gitbook/assets/image (264).png>)

![](<../../../../.gitbook/assets/image (293).png>)

![](<../../../../.gitbook/assets/image (305).png>)

## Testing

The Scheduled Task runs perfectly as an Administrator in the GUI, and it can also be run in PowerShell by usin&#x67;**`Start-ScheduledTask`**. You will need to specify the **`TaskPath`** if the Task is not in the ROOT of Task Scheduler, or **`Get-ScheduledTask`** and pipe it t&#x6F;**`Start-ScheduledTask`**

![](<../../../../.gitbook/assets/image (341).png>)

![](<../../../../.gitbook/assets/image (335).png>)

When logged in as a Standard User, the Task will not show up. This is due to the fact that the Task does not have permissions to run in a Standard User profile

![](<../../../../.gitbook/assets/image (337).png>)

![](<../../../../.gitbook/assets/image (421).png>)

The reason the Task does not show for the Standard User is that the Standard User does not have READ and EXECUTE permissions to the Scheduled Task. Time to read the next page

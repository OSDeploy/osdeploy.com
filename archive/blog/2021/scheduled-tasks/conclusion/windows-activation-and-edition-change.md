# Windows Activation and Edition Change

I work for an Oil Services company, and while most of our computers are Domain Joined and connected to our Corporate Network, we have several computers that are Remote and in Workgroups.  These computers can't readily talk to our KMS to stay Activated as they are Offline for extended periods of time

The need to create a Scheduled Task to sit on a computer for Activating Windows using a MAK (Multiple Activation Key) is necessary.

## Activation Script

Below is a quick PowerShell script to activate Windows 10 Enterprise using a MAK.  Simply replace the **`$MakKey`** with your own

```
$TaskName = 'Activate MAK Windows 10 Enterprise'
$MakKey = 'XXXXX-XXXXX-XXXXX-XXXXX-XXXXX'
#======================================================================================
#   Logs
#======================================================================================
$TaskLogs = "$env:SystemRoot\Logs\Activation"
if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
$TaskLogName = "$((Get-Date).ToString('yyyy-MM-dd-HHmmss'))-$TaskName.log"
Start-Transcript -Path (Join-Path $TaskLogs $TaskLogName)
#======================================================================================
#   Operating System
#======================================================================================
$OSCaption = $((Get-WmiObject -Class Win32_OperatingSystem).Caption).Trim()
$OSArchitecture = $((Get-WmiObject -Class Win32_OperatingSystem).OSArchitecture).Trim()
$OSVersion = $((Get-WmiObject -Class Win32_OperatingSystem).Version).Trim()
$OSBuildNumber = $((Get-WmiObject -Class Win32_OperatingSystem).BuildNumber).Trim()
#======================================================================================
#   Variables
#======================================================================================
Write-Host "OSCaption: $OSCaption" -ForegroundColor Cyan
Write-Host "OSArchitecture: $OSArchitecture" -ForegroundColor Cyan
Write-Host "OSVersion: $OSVersion" -ForegroundColor Cyan
Write-Host "OSBuildNumber: $OSBuildNumber" -ForegroundColor Cyan
#======================================================================================
#   Set SLMGR
#======================================================================================
if (Test-Path "$env:windir\SYSWOW64\slmgr.vbs") {
    $slmgr = "$env:windir\SYSWOW64\slmgr.vbs"
} else {
    $slmgr = "$env:windir\System32\slmgr.vbs"
}
#======================================================================================
#   OS Activation
#======================================================================================
if (Test-Path $slmgr) {
    Write-Host "**********************"
    Write-Host "Display Licensing Information"
    Write-Host "Command Line: cscript //nologo $slmgr /dlv"
    cscript //nologo $slmgr /dlv
    Write-Host "**********************"
    Write-Host "Install Product Key"
    Write-Host "Command Line: cscript //nologo $slmgr /ipk $MakKey"
    cscript //nologo $slmgr /ipk $MakKey
    Write-Host "**********************"
    Write-Host "Activate Windows"
    Write-Host "Command Line: cscript //nologo $slmgr /ato"
    cscript //nologo $slmgr /ato
    Write-Host "**********************"
    Write-Host "Display Licensing Information"
    Write-Host "Command Line: cscript //nologo $slmgr /dlv"
    cscript //nologo $slmgr /dlv
    Write-Host "**********************"
    Write-Host "Display Installation ID for Offline Activation"
    Write-Host "Command Line: cscript //nologo $slmgr /dti"
    cscript //nologo $slmgr /dti
}
Write-Host "**********************"
Write-Warning "Internet access may be required to complete activation"
#======================================================================================
#   Complete
#======================================================================================
Stop-Transcript
```

## Inception

![](<../../../../../.gitbook/assets/image (284).png>)

You are going to have to take this script, and put in in your Task Builder script to create the Task so it can be Encoded

```
#======================================================================================
#   Run as Administrator Elevated
if (!([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Verbose "Checking User Account Control settings" -Verbose
    if ((Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System).EnableLUA -eq 0) {
        #UAC Disabled
        Write-Verbose "User Account Control is Disabled" -Verbose
        Write-Verbose "You will need to correct your UAC Settings before running this script" -Verbose
        Write-Verbose "Try running this script in an Elevated PowerShell session ... Exiting" -Verbose
        Start-Sleep -s 10
        Exit 0
    } else {
        #UAC Enabled
        Write-Verbose "UAC is Enabled. Relaunching script with Elevated Permissions" -Verbose
        Start-Process powershell.exe "-NoProfile -ExecutionPolicy Bypass -File `"$PSCommandPath`"" -Verb RunAs -Wait
        Exit 0
    }
} else {
    Write-Verbose "Running with Elevated Permissions" -Verbose
}
#======================================================================================
#   Task Properties
$TaskName = 'Activate MAK Windows 10 Enterprise'
$TaskPath = '\Corporate\Activation'
$Description = @"
Upgrades Edition to Windows 10 Enterprise  
Activates Windows using Multiple Activation Key 
Internet access may be required to complete the Activation Process  
Transcripts are stored in $env:SystemRoot\Logs\Activation  
Runs as SYSTEM and does not display any progress or results  
PowerShell Encoded Script  
Version 21.1.19
"@
#======================================================================================
#   Script
$TaskScript = @'
$TaskName = 'Activate MAK Windows 10 Enterprise'
$MakKey = 'XXXXX-XXXXX-XXXXX-XXXXX-XXXXX'
#======================================================================================
#   Logs
#======================================================================================
$TaskLogs = "$env:SystemRoot\Logs\Activation"
if (!(Test-Path $TaskLogs)) {New-Item $TaskLogs -ItemType Directory -Force | Out-Null}
$TaskLogName = "$((Get-Date).ToString('yyyy-MM-dd-HHmmss'))-$TaskName.log"
Start-Transcript -Path (Join-Path $TaskLogs $TaskLogName)
#======================================================================================
#   Operating System
#======================================================================================
$OSCaption = $((Get-WmiObject -Class Win32_OperatingSystem).Caption).Trim()
$OSArchitecture = $((Get-WmiObject -Class Win32_OperatingSystem).OSArchitecture).Trim()
$OSVersion = $((Get-WmiObject -Class Win32_OperatingSystem).Version).Trim()
$OSBuildNumber = $((Get-WmiObject -Class Win32_OperatingSystem).BuildNumber).Trim()
#======================================================================================
#   Variables
#======================================================================================
Write-Host "OSCaption: $OSCaption" -ForegroundColor Cyan
Write-Host "OSArchitecture: $OSArchitecture" -ForegroundColor Cyan
Write-Host "OSVersion: $OSVersion" -ForegroundColor Cyan
Write-Host "OSBuildNumber: $OSBuildNumber" -ForegroundColor Cyan
#======================================================================================
#   Set SLMGR
#======================================================================================
if (Test-Path "$env:windir\SYSWOW64\slmgr.vbs") {
    $slmgr = "$env:windir\SYSWOW64\slmgr.vbs"
} else {
    $slmgr = "$env:windir\System32\slmgr.vbs"
}
#======================================================================================
#   OS Activation
#======================================================================================
if (Test-Path $slmgr) {
    Write-Host "**********************"
    Write-Host "Display Licensing Information"
    Write-Host "Command Line: cscript //nologo $slmgr /dlv"
    cscript //nologo $slmgr /dlv
    Write-Host "**********************"
    Write-Host "Install Product Key"
    Write-Host "Command Line: cscript //nologo $slmgr /ipk $MakKey"
    cscript //nologo $slmgr /ipk $MakKey
    Write-Host "**********************"
    Write-Host "Activate Windows"
    Write-Host "Command Line: cscript //nologo $slmgr /ato"
    cscript //nologo $slmgr /ato
    Write-Host "**********************"
    Write-Host "Display Licensing Information"
    Write-Host "Command Line: cscript //nologo $slmgr /dlv"
    cscript //nologo $slmgr /dlv
    Write-Host "**********************"
    Write-Host "Display Installation ID for Offline Activation"
    Write-Host "Command Line: cscript //nologo $slmgr /dti"
    cscript //nologo $slmgr /dti
}
Write-Host "**********************"
Write-Warning "Internet access may be required to complete activation"
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

## Create the Task

Simply execute the script to create the Scheduled Task.  In my example, I have created one for Enterprise and Pro as I work with both editions in my environment.  Each Edition should have a separate, unique MAK Key

![](<../../../../../.gitbook/assets/image (213).png>)

![](<../../../../../.gitbook/assets/image (194).png>)

## Standard User Testing

So now I'm on the Internet completely disconnected from my Corporate Network and I need to Activate Windows using the MAK, so time to Change Product Key in Windows Settings

![](<../../../../../.gitbook/assets/image (179).png>)

Unfortunately I am not an Admin of the computer, so no can do

![](<../../../../../.gitbook/assets/image (163).png>)

Time to run the Scheduled Task and wait for it to complete running.  Another check into Windows Activation and I am golden.

![](<../../../../../.gitbook/assets/image (307).png>)

I'm able to alternate both of my Tasks and switch between Enterprise and Pro

![](<../../../../../.gitbook/assets/image (261).png>)

The logging is a plus and very helpful if there are any issues that come up

![](<../../../../../.gitbook/assets/image (349).png>)

## GitHub

Here's a link to both scripts.  Just add your MAK Keys and enjoy

{% embed url="https://github.com/OSDeploy/ScheduledTasks/tree/main/Corporate/Activation" %}

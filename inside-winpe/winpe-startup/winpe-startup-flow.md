# WinPE Startup Flow

We've taken a look at two steps of the WinPE Startup flow, but there are a few more steps in this process

{% stepper %}
{% step %}
#### Registry: SYSTEM\Setup\CmdLine

Winlogon reads the value of CmdLine, which is winpeshl.exe
{% endstep %}

{% step %}
#### Windows PE Shell (winpeshl.exe)

Initializes PNP and WallpaperHost.exe

Executes commands from winpeshl.ini but this is skipped as this file doesn't exist. MDT would utilize this method for WinPE startup.

Tries to execute \[SystemDrive]\\$Windows.\~BT\sources\setup.exe (does not exist)

Tries to execute \[SystemDrive]\setup.exe (does not exist)

Executes \[SystemDrive]\windows\system32\cmd.exe /k startnet.cmd
{% endstep %}

{% step %}
#### Startnet.cmd

Executes wpeinit.exe and remains as the shell application. This is the Command Prompt window that is left open. When the Command Prompt window is closed, WinPE will restart
{% endstep %}

{% step %}
#### Wpeinit.exe

Completes the WinPE initialization and Unattend.xml
{% endstep %}
{% endstepper %}

## Additional Reading

{% embed url="https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc721977(v=ws.10)" %}

{% embed url="https://oofhours.com/2020/12/03/windows-pe-startup-revisited/" %}

{% embed url="https://oofhours.com/2021/01/17/build-your-own-windows-pe-image/" %}

{% embed url="https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc721977(v=ws.10)" %}

{% embed url="https://slightlyovercomplicated.com/2016/11/07/windows-pe-startup-sequence-explained/" %}

# Wpeinit.exe

We now know that wpeinit is called by the Startnet. But what does this executable actually do?

## About Wpeinit

{% embed url="https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/wpeinit-and-startnetcmd-using-winpe-startup-scripts?view=windows-11" %}

Microsoft learn has a very simple page dedicated to Wpeinit. What it does tell us is that Wpeinit outputs log messages to "C:\Windows\system32\wpeinit.log", but that is incorrect when booted to WinPE, we actually need to look in the SystemDrive, which is X:\\

We can easily open this file in notepad.exe and get an understanding of what Wpeinit does

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

In simple terms, Wpeinit is used to start the WinPE environment, optionally loading settings found in X:\Unattend.xml. In absence of an Unattend.xml, default settings are used.

{% stepper %}
{% step %}
### Loads settings from X:\Unattend.xml

This file does not exist by default, and must be added to the mounted boot.wim
{% endstep %}

{% step %}
### Display Settings

Unattend.xml: [Microsoft-Windows-Setup/Display](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-display)

On UEFI-based computers, this setting is ignored unless you have added a graphics driver into the Windows PE image at \sources\boot.wim. On BIOS-based computers, by default, Windows PE uses a resolution of 1024x768. For displays with a smaller maximum resolution, Windows PE uses the largest resolution available, based on the extended display identification data (EDID) of the connected display.
{% endstep %}

{% step %}
### Computer Name

Random computer name is generated, then winmgmr (WMI) is started
{% endstep %}

{% step %}
### Virtual Memory Paging File

Unattend.xml: [Microsoft-Windows-Setup/PageFile](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-pagefile)

Specifies the location and the size of the Windows PE operating system page file to create. This setting applies only to the Windows PE page file, and not to the page file of the Windows installation.
{% endstep %}

{% step %}
### Optional Components

ADK WinPE OC's such as WMI and WSH are initialized if present
{% endstep %}

{% step %}
### Network Access and Applying Configuration

Unattend.xml: [Microsoft-Windows-Setup/EnableNetwork](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-enablenetwork)

Specifies whether network connection is enabled.
{% endstep %}

{% step %}
### Firewall Settings

Unattend.xml: [Microsoft-Windows-Setup/EnableFirewall](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-enablefirewall)

Specifies whether Windows Firewall is enabled for Windows Preinstallation Environment (Windows PE). This setting does not apply to the Windows Firewall settings of the Windows installation.
{% endstep %}

{% step %}
### Synchronous User-Provided Commands

Unattend.xml: [Microsoft-Windows-Setup/RunSynchronous](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-runsynchronous)

Specifies one or more commands to run during the windowsPE configuration pass.

Synchronous commands start in the order that you specify in the unattended installation answer file, and each command must finish before the next command starts.

To start a command that needs to finish before other commands can start, use synchronous commands. To run services or commands that can start at the same time, use RunAsynchronous instead. RunSynchronous commands always run before RunAsynchronous commands in the same pass.
{% endstep %}

{% step %}
### Asynchronous User-Provided Commands

Unattend.xml: [Microsoft-Windows-Setup/RunAsynchronous](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-runasynchronous)

Specifies one or more commands to run during the windowsPE configuration pass.

To run services or commands that can start at the same time, use asynchronous commands. To run commands that need to finish before other commands can start, use RunSynchronous instead.
{% endstep %}

{% step %}
### Shutdown Settings

Unattend.xml: [Microsoft-Windows-Setup/Restart](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-restart)

Specifies whether to restart or to shut down the computer after the windowsPE configuration pass. This setting is specific to Windows PE scenarios. Once image-based installation begins, this setting is ignored.
{% endstep %}
{% endstepper %}

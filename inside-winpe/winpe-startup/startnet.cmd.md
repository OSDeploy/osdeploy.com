# Startnet.cmd

When WinPE starts, it launches an Administrator command prompt with a single execution of wpeinit.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

So where does this startup sequence come from? Microsoft Learn docs answer this question for us

{% embed url="https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/wpeinit-and-startnetcmd-using-winpe-startup-scripts?view=windows-11" %}

> By default, Windows PE includes a Startnet.cmd script located at %SYSTEMROOT%\System32 of your customized Windows PE image.
>
> Startnet.cmd starts Wpeinit.exe. Wpeinit.exe installs Plug and Play devices, processes Unattend.xml settings, and loads network resources.

## Content

From the command prompt, you can use notepad to view the content of Startnet.cmd. It is this file that launches wpeinit.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

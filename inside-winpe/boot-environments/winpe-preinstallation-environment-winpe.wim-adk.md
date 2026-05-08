# WinPE - Preinstallation Environment (winpe.wim) ADK

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Boot Process

{% stepper %}
{% step %}
#### winlogon.exe

```
HKEY_LOCAL_MACHINE\SYSTEM\Setup\CmdLine
REG_SZ
winpeshl.exe
```
{% endstep %}

{% step %}
#### winpeshl.exe

```
YYYY-MM-DD HH:mm:ss.SSS, Info    Windows PE Shell beginning execution
YYYY-MM-DD HH:mm:ss.SSS, Info    Beginning PNP initialization.
YYYY-MM-DD HH:mm:ss.SSS, Info    Succeeded launching application X:\windows\system32\WallpaperHost.exe [command line: X:\windows\system32\WallpaperHost.exe]
YYYY-MM-DD HH:mm:ss.SSS, Info    PNP initialization succeeded; terminating thread.
YYYY-MM-DD HH:mm:ss.SSS, Warning Failed to launch application (null) [command line: X:\$Windows.~BT\sources\setup.exe] [0x80070002]
YYYY-MM-DD HH:mm:ss.SSS, Warning Failed to launch application (null) [command line: x:\setup.exe] [0x80070002]
YYYY-MM-DD HH:mm:ss.SSS, Info    Succeeded launching application (null) [command line: X:\windows\system32\cmd.exe /k startnet.cmd]
```
{% endstep %}

{% step %}
#### startnet.cmd

```bat
wpeinit

```
{% endstep %}
{% endstepper %}

## Wallpapers

{% stepper %}
{% step %}
#### winpe.jpg

<figure><img src="../../.gitbook/assets/winpe (1).jpg" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## Content

{% file src="../../.gitbook/assets/content-WinSxS.txt" %}
WinSxS removed
{% endfile %}

{% file src="../../.gitbook/assets/content.txt" %}

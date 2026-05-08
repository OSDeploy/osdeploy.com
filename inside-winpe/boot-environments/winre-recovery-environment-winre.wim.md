# WinRE - Recovery Environment (winre.wim)

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

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
YYYY-MM-DD HH:mm:ss.SSS, Info    Winpeshl.ini detected.
YYYY-MM-DD HH:mm:ss.SSS, Info    [LaunchApp]: Launching [X:\sources\recovery\recenv.exe]
YYYY-MM-DD HH:mm:ss.SSS, Info    Succeeded launching application (null) [command line: X:\sources\recovery\recenv.exe]
YYYY-MM-DD HH:mm:ss.SSS, Info    PNP initialization succeeded; terminating thread.
```
{% endstep %}

{% step %}
#### recenv.exe
{% endstep %}
{% endstepper %}

## Wallpapers

{% stepper %}
{% step %}
#### winpe.jpg

<figure><img src="../../.gitbook/assets/winpe.jpg" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### winre.jpg

<figure><img src="../../.gitbook/assets/winre.jpg" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## Content

{% file src="../../.gitbook/assets/content-WinSxS (3).txt" %}
WinSxS removed
{% endfile %}

{% file src="../../.gitbook/assets/content (3).txt" %}

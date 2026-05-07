# I Hate OSDBuilder

While sitting in an MMSJazz session last Tuesday, I had some time for reflection.  I remember the old days of Johan and Mikael's Geek Week, and how things have changed.  Here I sat, looking at OSDBuilder ... that rough brute raw sexiness of PowerShell Functions telling me what and where to do this and that.  I miss the gentle touch of mounting my own WIMs ... and the finesse of adding the Updates manually.  The forgotten craft of doing everything by hand.  Bespoke.

"Back to the Basics" was the session, or something like that ... but OSDBuilder is lost, adding more and more until I no longer recognize my old friend.  But with MMSJazz came an opportunity to meet new friends, and to spend a little time creating a new one.

## OSD Module

So I've been working on a new PowerShell module with some friends called OSD.  If you've been using OSDBuilder, then you already have it.  You'll need to give it an update first

```
Update-Module OSD -Force
Import-Module OSD -Force
```

Ok, now how about a proper introduction.  There it is, version 19.11.19.0.  Keep in mind this is a work in progress for now ...

![](<../../../../.gitbook/assets/image (132).png>)

## Say Hello To My Little Friend

So I decided to mount a WinPE.wim and run a quick check using the new **`Update-OSDWindowsImage`** function

```
Get-WindowsImage -Mounted | Update-OSDWindowsImage -CheckOnly
```

![](<../../../../.gitbook/assets/image (135).png>)

Well that wasn't too pretty ... but it did know exactly what updates I need to install (with the help of the OSDSUS Module).  Maybe I should update the SSU

```
Get-WindowsImage -Mounted | Update-OSDWindowsImage -UpdateGroup SSU
```

![](<../../../../.gitbook/assets/image (128).png>)

That worked out quite well, it found the SSU Update, downloaded it, and installed too!  Time to see if I can update everything using Bits Transfer on the download

```
Get-WindowsImage -Mounted | Update-OSDWindowsImage -BitsTransfer
```

![](<../../../../.gitbook/assets/image (136).png>)

Well that was supercool ... but what if I want to install an update again?  I guess I need to Force it!

```
Get-WindowsImage -Mounted | Update-OSDWindowsImage -UpdateGroup SSU -Force
```

![](<../../../../.gitbook/assets/image (123).png>)

## Big Wim Time

Ok, so that was WinPE, where things are quite simple.  What about an Install.wim?

![](<../../../../.gitbook/assets/image (141).png>)

Ok, I wasn't expecting Adobe to be installed ... luckily Update-OSDWindowsImage sets a Global Variable for me to check if that is accurate

```
$GetOSDSessions
```

![](<../../../../.gitbook/assets/image (127).png>)

I guess it was installed.  I'm short on time and need to skip the LCU Update so I'll focus on the SSU and 2 DotNetCU Updates

```
Get-WindowsImage -Mounted | Update-OSDWindowsImage -UpdateGroup SSU
Get-WindowsImage -Mounted | Update-OSDWindowsImage -UpdateGroup DotNetCU
Get-WindowsImage -Mounted | Update-OSDWindowsImage -CheckOnly
```

![](<../../../../.gitbook/assets/image (126).png>)

## The More The Merrier

Hmm, but I was passing Get-WindowsImage -Mounted to my new friend.  I wonder what will happen if I have two images mounted ... and to make things more difficult, I'm going to make the second one Windows 10 1809 x86

![](<../../../../.gitbook/assets/image (142).png>)

Looks good to me!  Well it's time to go now that I've met someone new, but I need to cut things short so I can spend the next few hours filling out my 5th MVP Nomination (thanks Damien)

{% hint style="warning" %}
**OSD Module is still under development, but things are starting to look pretty good ;)**
{% endhint %}

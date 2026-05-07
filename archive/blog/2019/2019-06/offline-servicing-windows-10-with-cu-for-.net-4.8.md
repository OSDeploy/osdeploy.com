# Offline Servicing Windows 10 with CU for .NET 4.8

I received a question in email concerning the .NET Cumulative Updates&#x20;

> Thanks for your incredible modules. I’m just getting started with them, but really like them so far. I work for a hospital and unfortunately we have applications that are not yet compatible with .NET 4.8. In fact, I believe we are currently locked at 4.7.1. When I run the “Update-OSMedia” step, for Windows 10 v1709 Enterprise, I am only presented with updates for v4.8

Checking OSDBuilder's **`Update-OSMedia`**, yes there are Cumulative Updates for .NET Framework 4.8 that are applied

![](<../../../../.gitbook/assets/image (444).png>)

### Is .NET Framework 4.8 Installed

The easiest method is to check if .NET Framework 4.8 is installed is to check for the Release value in the Registry

```
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full\' |  Get-ItemPropertyValue -Name Release
```

On Windows 10 1709 with full Updates to June 27, 2019, **Release 461308** is returned

![](<../../../../.gitbook/assets/image (447).png>)

A quick check in the .NET Framework Version shows that Windows 10 1709 as .NET Framework 4.7.1 installed, **so no, .NET Framework 4.8 is not installed**

{% embed url="https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed#version_table" %}

![](<../../../../.gitbook/assets/image (443).png>)

### Check the .NET Framework Cumulative Update's Log

You could easily see by checking the Log that the Update is not installed and marked 'Absent'.  This is because it does not meet the requirements for installation ... because .NET Framework 4.8 is not installed

![](<../../../../.gitbook/assets/image (446).png>)

### Conclusion

**Installing a Cumulative Update for .NET Framework 4.8 only installs if you have .NET Framework 4.8 installed.**

### More Clarity

After sharing this post with the person that send me the original question, I received a reply

> I read your post, but I don’t feel it provided an answer that I was looking for. Most likely I did not express my question accurately. We knew 4.7.1 was installed out of the box with 1709. We just have special requirements to keep .NET at that version, but patched.

**Let me be absolutely clear**

* Installing a Cumulative Update for .NET Framework 4.8 in Windows (online or offline) will only update the .NET Framework 4.8 files
* If you do not have .NET Framework 4.8, it will not update anything
* If you have .NET Framework 4.7.1, it will not upgrade it to .NET Framework 4.8

### So if the Updates are not actually installing, then why have them in OSDBuilder?

Because I'm lazy.

What this means is that I dedicate sometimes several hours a week keeping new Updates in OSDBuilder.  I do not have the time to test every Update on every Windows version and review the log files to see what is actually being installed and what is not.  Updates can change weekly.  I am not willing to do this because I don't want to dedicate 10 hours a week to checking every Update.  I have a job, a family, and a life.

And yes I did spend some length of time writing this post so it can be fully understood.  Rather then just answer the person that emailed, I prefer to write this post so others with the same question can understand.

### The Short Answer

**If you want .NET Framework 4.8 installed, you need to do this in Windows, not through Offline Servicing**

# WinPE Problems

So you now have a WinPE Boot Image with PowerShell installed, everything works perfectly, as long as you stay contained working only with what's already installed and enabled in WinPE. The problem starts when you want to step out of the box and try to do something as basic as installing a PowerShell Module from the PowerShell Gallery.

## PackageManagement is not installed

In the example below, this process fails because default ADK WinPE does not include PackageManagement.&#x20;

<figure><img src="../.gitbook/assets/image (705).png" alt=""><figcaption></figcaption></figure>

## PowerShell ExecutionPolicy is Restricted

Now let's say by some magic that you were able to get PackageManagement installed in your WinPE Boot Image, your next issue is that WinPE's default Execution Policy is Restricted. This is an easy fix, and there should be no issues setting this policy to Bypass.

<figure><img src="../.gitbook/assets/image (706).png" alt=""><figcaption></figcaption></figure>

## Environment Variables

Now that the Execution Policy is resolved, another roadblock. The cause of the errors in the screenshot below are caused by `$env:LOCALAPPDATA`

<figure><img src="../.gitbook/assets/image (707).png" alt=""><figcaption></figcaption></figure>

Environment Variables can be displayed with the following PowerShell commnd

```powershell
Get-ChildItem env:
```

After running this command, its clear that a few Environment Variables and their values don't exist

<figure><img src="../.gitbook/assets/image (709).png" alt=""><figcaption></figcaption></figure>

```
APPDATA = X:\windows\system32\config\systemprofile\AppData\Roaming
HOMEDRIVE = X:
HOMEPATH = \windows\system32\config\systemprofile
LOCALAPPDATA = X:\windows\system32\config\systemprofile\AppData\Local
```

Easy enough to fix

<figure><img src="../.gitbook/assets/image (710).png" alt=""><figcaption></figcaption></figure>

## PowerShellGet is not up to date

The next problem with a simple Install-Module is because there are no PSRepositories, which is because PowerShellGet is version 1.0.0.1 and needs to be updated

<figure><img src="../.gitbook/assets/image (711).png" alt=""><figcaption></figcaption></figure>

## Register PSRepository

Importing the PowerShellGet Module isn't enough, PSGallery needs to be registered

<figure><img src="../.gitbook/assets/image (712).png" alt=""><figcaption></figcaption></figure>

Making sure that we are using the new PowerShellGet version as the PackageProvider

<figure><img src="../.gitbook/assets/image (713).png" alt=""><figcaption></figcaption></figure>

## PSGallery InstallationPolicy Trusted

Finally, the last piece of the puzzle is to set the Installation Policy to Trusted to make sure we can script this solution

```powershell
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
```

<figure><img src="../.gitbook/assets/image (714).png" alt=""><figcaption></figcaption></figure>

## PowerShell Profile

While this issue didn't prevent us from installing a module from PowerShell Gallery, its important to note that the PowerShell Profile for Current User is broken.

<figure><img src="../.gitbook/assets/image (716).png" alt=""><figcaption></figcaption></figure>

Typically, this would be the following path

```
$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

This can easily be resolved by setting these variables.

<figure><img src="../.gitbook/assets/image (717).png" alt=""><figcaption></figcaption></figure>

## User Shell Folders

The root cause of the PowerShell Profile issue is that some of the User Shell Folders do not exist, so you should make sure these paths exist and create them in your WinPE if they do not

```
$env:UserProfile\AppData\Local
$env:UserProfile\AppData\Roaming
$env:UserProfile\Desktop
$env:UserProfile\Documents\WindowsPowerShell
```

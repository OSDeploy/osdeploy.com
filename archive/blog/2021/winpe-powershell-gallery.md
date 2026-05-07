---
description: March 8, 2021
---

# PowerShell Gallery in WinPE

It would be nice if you could install PowerShell Modules in WinPE from PowerShell Gallery ... here is how

## No PackageManagement <a href="#no-packagemanagement" id="no-packagemanagement"></a>

The first problem is that even though PowerShellGet is installed in WinPE, PackageManagement is not. This leads to a Chicken and Egg situation where you can't install PackageManagement from PowerShell Gallery until PackageManagement is installed

![](https://gblobscdn.gitbook.com/assets%2F-LpnxLqvh8u2fEz86kIM%2F-MVIO0PvSWQ65xUI578M%2F-MVIPRf9hTo7Awbvv6Ul%2Fimage.png?alt=media\&token=9da56bd3-75c2-4391-8602-9aaef5f16ccb)

## No LocalAppData <a href="#no-localappdata" id="no-localappdata"></a>

So you manage to get a copy of the **`PackageManagement`** PowerShell Module saved in your WinPE WIM, but things still don't work. This is because WinPE does not contain a Volatile Environment at **`HKEY_CURRENT_USER\Volatile Environment`**

![](https://gblobscdn.gitbook.com/assets%2F-LpnxLqvh8u2fEz86kIM%2F-MVIO0PvSWQ65xUI578M%2F-MVIPuqhAccnrxc9kAkB%2Fimage.png?alt=media\&token=354fc3d9-e046-4fe8-80b8-d3536646e19e)

Specifically, what is missing is **`LOCALAPPDATA`**

![](https://gblobscdn.gitbook.com/assets%2F-LpnxLqvh8u2fEz86kIM%2F-MVIO0PvSWQ65xUI578M%2F-MVIQYFBZw3Sj-cM_VsR%2Fimage.png?alt=media\&token=f7816480-cccf-4803-b6a6-cc54ba81163d)

## The Function <a href="#the-function" id="the-function"></a>

I ended up writing a function that will make these changes for you.  Feel free to have a look

{% embed url="https://osd.osdeploy.com/module/functions/winpe/enable-pewimpsgallery" %}

What this function does is

1. Mounts a WinPE WIM
2. Adds the missing Volatile Environment (in Current Environment)
3. Saves PackageManagenment and PowerShellGet Modules from PowerShell Gallery in the WIM
4. Dismount and Save the WinPE WIM

![](https://gblobscdn.gitbook.com/assets%2F-LpnxLqvh8u2fEz86kIM%2F-MVIO0PvSWQ65xUI578M%2F-MVIUO1NqkGYLzOjrEVa%2Fimage.png?alt=media\&token=739a08b2-5a40-412b-985a-62e9eb3f37aa)

## Results <a href="#results" id="results"></a>

After modifying the WIM, I can test the changes by creating an ISO (or you can USB, whatever). Works like a champ!

![](https://gblobscdn.gitbook.com/assets%2F-LpnxLqvh8u2fEz86kIM%2F-MVIO0PvSWQ65xUI578M%2F-MVIVpUDgScKG1LTeNve%2Fimage.png?alt=media\&token=aef47baa-0c5c-415e-a162-901f363663c2)

![](https://gblobscdn.gitbook.com/assets%2F-LpnxLqvh8u2fEz86kIM%2F-MVIO0PvSWQ65xUI578M%2F-MVIX-A-oyYqoyz9fnP9%2Fimage.png?alt=media\&token=3a1ecec2-5b50-4e67-b421-3e5076308986)

​

​

​

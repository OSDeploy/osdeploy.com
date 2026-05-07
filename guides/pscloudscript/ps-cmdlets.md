---
description: 22.1.16
---

# PS Cmdlets

The two PowerShell Cmdlets that are used are `Invoke-RestMethod`and `Invoke-Expression`.  It is also important that these cmdlets are processed in this order, which I will touch on at the bottom of this page

## Invoke-RestMethod

This cmdlet is used to return the contents of a provided URL in a structured, typically Raw format

{% embed url="https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-5.1" %}

Here are some examples of how to use Invoke-RestMethod to get the raw content from a Url

```powershell
PS C:\> Invoke-RestMethod -Uri https://raw.githubusercontent.com/OSDeploy/PSCloudScript/main/ps-cmdlets.txt
This is a test to see if this raw content can be saved as a PowerShell string

PS C:\> irm https://raw.githubusercontent.com/OSDeploy/PSCloudScript/main/ps-cmdlets.txt
This is a test to see if this raw content can be saved as a PowerShell string

PS C:\> $String = irm raw.githubusercontent.com/OSDeploy/PSCloudScript/main/ps-cmdlets.txt
PS C:\> $String
This is a test to see if this raw content can be saved as a PowerShell string
```

## Invoke-Expression

This cmdlet is used to execute a PowerShell Command.  You can read Microsoft's documentation here

{% embed url="https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-5.1" %}

If you read the Microsoft Doc, you'll understand that `iex` commands can come from a `String`.  This can be easily demonstrated in this example

```powershell
PS D:\> $String = @'
Write-Host 'This is PowerShell String'
'@

PS D:\> Invoke-Expression -Command $String
This is PowerShell String

PS D:\> Invoke-Expression $String
This is PowerShell String

PS D:\> iex $String
This is PowerShell String
```

This method even works if there is a PowerShell function in the String.  Keep in mind this function is only available in the current PowerShell session

```powershell
PS D:\> $StringFunction = @'
function Test-PowerShell
{
    [CmdletBinding()]
    param()
    Write-Host 'This is a PowerShell Function'
    Write-Verbose 'And this is a Verbose PowerShell Function'
}
'@

PS D:\> Invoke-Expression -Command $StringFunction

PS D:\> Test-PowerShell
This is a PowerShell Function

PS D:\> Test-PowerShell -Verbose
This is a PowerShell Function
VERBOSE: And this is a Verbose PowerShell Function
```

## Order

These commands have to follow a specific order.  You need the content from `Invoke-RestMethod` before you can execute in `Invoke-Expression`

```
#Command in parenthesis is processed first in both of these examples
Invoke-Expression -Command (Invoke-RestMethod -Uri $Uri)
#Alternate using Alias
iex(irm $Uri)

#Pipeline can be used
Invoke-RestMethod -Uri $Uri | Invoke-Expression
#Alternate using Alias
irm $Uri | iex
```

## Sponsor

OSDeploy is sponsored by [Recast Software](https://www.recastsoftware.com/?utm_source=osdeploy\&utm_medium=ad\&utm_campaign=web) and their Systems Management Tools

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}
Sponsored by Recast Software
{% endembed %}

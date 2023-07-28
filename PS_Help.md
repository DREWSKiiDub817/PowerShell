# Getting Help

## Getting Help Introduction
- **Get-Help**
```PowerShell
# get-help usage
Get-Help -Name Get-Service
Get-Help Get-Service -Examples
Get-Help Get-Service -Detailed
Get-Help Get-Service -Full
Get-Help Get-Service -ShowWindow

# help topics available that end in "service"
Get-Help *service

Get-Help -Name Test-Connection -Detailed
Get-Help -Name Test-Connection -Parameter IP*

# about topics (PS concepts)
Get-Help about_*
Get-Help about_Scripts

########## Save-Help.ps1 ##########

#download help topics to a file share
Save-Help -DestinationPath \\FileSharePath\PSHelp -UICulture en-US -ErrorAction SilentlyContinue

#retrieve server names from Active Directory
$servers = Get-ADComputer -Filter {OperatingSystem -like "*server*"} |
    Select-Object -ExpandProperty Name

# send update help command to every server
Invoke-Command -ComputerName $servers -ScriptBlock {
    Update-Help -SourcePath \\FileSharePath\PSHelp -ErrorAction SilentlyContinue
}
```
- **Syntax and Parameters**
```PowerShell
# view help topic for Get-Random
Get-Help Get-Random
Get-Help Get-Random -Parameter *

# common parameters
[System.Management.Automation.Cmdlet]::CommonParameters
[System.Management.Automation.Cmdlet]::OptionalCommonParameters
Remove-Item -Path "\\FilePath\Data" -WhatIf

# view help topic for Get-ChildItem
Get-Help Get-Content

# Helpful Tip
Ctrl + Spacebar

```
- **Get-Command**
```PowerShell
# help topic for Get-Command
Get-Help -Name Get-Command

# get all cmdlets, functions, aliases on this machine
Get-Command

# get every type of command on this machine
Get-Command *

# get commands that contain a string
Get-Command *computer*

# get commands in current session
Get-Command -ListImported

# get commands by type
Get-Command -CommandType Alias
Get-Command dir -CommandType Alias

# get commands in a module, by verb, and by verb/noun
Get-Command -Module Microsoft.PowerShell.Management 
Get-Command -Module Microsoft.PowerShell.Management -Verb Get
Get-Command -Module Microsoft.PowerShell.Management -Verb Get -Noun C*

# get syntax for a command
Get-Command -Name Get-Content -Syntax

# get information about a command
Get-Command -Name Get-Content -ShowCommandInfo

# get commands that contain a specific parameter
Get-Command -ParameterName ComputerName

# get commands that contain a specific type of parameter
Get-Command -ParamterType PSCredential
Get-Command -ParamterType ServiceController
```

- **Get-Member** -
```PowerShell
# view help topic for Get-Member
Get-Help -Name Get-Member

# retrieve file object into variable
$file = Get-ChildItem "\\FilePath\Stuff\file.txt"

# view Get-Child object members
Get-Member -InputObject $file
$file | Get-Member

# view specific object members
$file | Get-Member -MemberType Properties
$file | Get-Member -MemberType Method

# view object property
$file.FullName

# execute object method
$file.CopyTo("C:\FilePath\File.txt")

# add a new member
$file | Add-Member -MemberType NoteProperty -Name "Owner" -Value "Whiskers"

# view specific property types
$file | Get-Member -View Extended

# view static members of a class
[DateTime] | Get-Member -Static
[DateTime]::MinValue
[DateTime]::MaxValue


## Get-Help CHALLENGE ##

# view the basic help topic for Get-Item
Get-Help -Name Get-Item
# identify the positional parameter for Get-Item
## The PATH parameter in the second parameter set is positional
Get-Item "\\FilePath\Data\File.txt" # <-example

# identify the required parameter for Get-Item
# The PATH parameter in the second parameter set is also required!
Get-Item # <-example (prompted for required params)

## Get-Command CHALLENGE ##

# find all commands in the ActiveDirectory module that end with user and computer in the noun
Get-Command -Module ActiveDirectory -Noun *computer, *user
# view the syntax for Get-ADComputer
Get-Command -Name Get-ADComputer -Syntax

# identify the default paramter set for Get-ADComputer
Get-Command -Name Get-ADComputer -ShowCommandInfo # filter param set is default

## Get-Member CHALLENGE ##

# view all members of Get-ADComputer
Get-ADComputer -Filter * | Get-Member

# view just the properties of the Get-ADUser
Get-ADUser -Filter * | Get-Member -MemberType Properties

```


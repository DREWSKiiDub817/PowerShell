
# PowerShell Basics

## PowerShell Components
- **Cmdlet** - Commands built into the shell
- **Parameter** - Arguments to cmdlet, function, script
- **Alias** - Short name for common cmdlets
- **Pipeline** - Pass objects between cmdlets
- **Function** - Commands written in PowerShell
- **Script** - PowerShell code stored in .ps1 file
- **Module** - Package of cmdlets, functions, etc.

</br>

- **Get-Service** to see a list of services on a system  
- **Set-Service** to configure service  
- **Restart-Service** to restart a service  
- **Get-ChildItem** gets the directory and child items in one or more specified locations  
- **Get-ChildItem \<path> -Recurse** return everything within that directory and all directory underneath   
- **-S** is the Alias for -Recurse
- **Get-Member** gets members, the properties and methods of objects
- **Get-Command** Gets all commands that are installed on the computer
 
## Objects and Members

- **3 Primary Members**  
   - **Properties**  
      - Attributes - describe a state of an object
   - **Methods**  
      - Actions - actions the object can perform 
   - **Events**  
      - Triggered Actions - when a certain action is taken against an object
- **Other Members**
  - AliasProperty - Alias for another property
  - NoteProperty - Hard-coded .NET object/value
  - ScriptProperty - Property extended with PowerShell
  - ScriptMethod - Method extended with PowerShell
  - CodeProperty - Property written in .NET
  - CodeMethod - Method written in .NET

## Variables and Data Types
- **Variables** 
  - store information
  - command results
  - **User** - User-defined variables
  - **Automatic** - PowerShell variables
  - **Preference** - User preference variables
 - **Data Type**
  - Type of Value within a variable
```PowerShell
# read variables
$num
$string
$object

# view data types
$num.GetType().Name
$string.GetType().Name
$object.GetType().Name
```

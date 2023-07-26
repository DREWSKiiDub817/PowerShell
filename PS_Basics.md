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

# explicit data type
[int]$num2 = 2
$num2 = "string"

# array
$arr = 1,2,3,"four",(Get-Item C:\Users\andrew.e.ara\Desktop)
$arr
$arr[1]
$arr[2,3]

# hashtable
$ht = @{1="one";2="two";3="three"}
$ht
$ht.1

# variables cmdlets
Get-Variable
New-Variable -Name "computer" -Value (Get-ADComputer -Identity "FS-NUG")
$computer
Get-Variable -Name "computer"
Remove=Variable -Name "computer"

# define variables
$g = "Good"
$d = "day"
$n = "drewskiidub"
$date = Get-Date

# single quotes (literal)
'$g $d, $n! Today is $date.'

# double quotes (evaluated)
"$g $d, $n! Today is $date."
# sub expression
"$g $d, $n! Today is $($date.DayOfWeek)."

```

## Operators
- **Arithmetic Operators**
  - **+** .... Adds values, concatenates strings
  - **-** .... Subtracts values
  - **\*** .... Multiply values, copy strings
  - **/** .... Divides values
  - **%** .... Modulus (remainder of values)
- **Assignment Operators**
  - **=** .... Set variable to value
  - **+=** .... Increase variable by value
  - **-=** .... Decrease variable by value
  - **\*=** .... Multiplies variable by value
  - **/=** .... Divides variable by value
  - **%=** .... Divides variable assigns modulus
- **Comparison Operators**
  - **-eq** .... Equals
  - **-ne** .... Not equals
  - **-gt** .... Greater than
  - **-lt** .... Less than
  - **-le** .... Less than or equal
  - **-ge** .... Greater than or equal
  - **-like** .... True when string matches
  - **-notlike** .... False when string matches
  - **-is** .... True if object is the same
  - **-isnot** .... True if objects are different
   
   ```PowerShell
   ## comparison operator
   Get-Process | 
      Where-Object { $_.WorkingSet -gt 100MB}
   ```
## Logical Operators
   - **-and** .... True when both conditions are true
   - **-or** .... True when either conditions are true
   - **-not** .... Negates a statement
   - **!** .... Same as -not
      ```PowerShell
      ## Logical operator
      Get-Process | 
         Where-Object { $_.WorkingSet -gt 100MB -and $_.WorkingSet -lt 200MB }
      ```

## Redirection Operators
   - **>** .... Send stream to a file
   - **>>** .... Append stream to a file
   - ***n*>&1** .... Redirects *n* to success stream
      ```PowerShell
      ## Redirection operator
      Get-Service -Name *win* > .\win-services.txt
      ```

## Split/Join Operators
   - **-split** .... Splits string into substrings
   - **-join** .... Concatenates strings together
      ```PowerShell
      ## Split operator
      -split "powershell 7 rocks!"
      ## join operator
      -join ("c:", "\", "Nuggetlab")
      ```
## Type Operators
   - **-is** .... True if .NET types are the same
   - **-isnot** .... True is .NET types are different
   - **-as** .... Converts to a .NET type
      ```PowerShell
      ## type operator
      # returns true
      (Get-Item -Path "C:\Nuggetlab") -is [System.IO.DirecotryInfo]
      # converts string to datetime
      "01/01/2020" -as [DateTime]
      ```
## Special Operator
   - **$()** .... Subexpression operator
   - **@()** .... Array subexpression operator
   - **&** .... Call operator
   - **::** .... Static member operator
   - **?:** .... Ternary operator
   - **??** .... Null-coalescing operator
   - **?.** .... Null-conditional operator
      ```PowerShell
      ## special operator
      # subexpression operator
      "Today is $(Get-Date)"
      # call operator
      $datecommand = "Get-Date"
      & $datecommand
      ```
## Putting it all Together!

```PowerShell
#### Get-MachineStats.ps1 ####

# ask for server name, store it in a variable
$server = Read-Host -Prompt "Enter a server name (localhost is default)"

# if no server name was entered, set the cariable to localhost
if ($server -eq "") {
      $server = "localhost"
}

# gather server stats
$os = Get-CimInstance Win32_OperatingSystem -ComputerName $server
$memTotal = [math]::Round($os.TotalVisibleMemorySize / 1MB, 2)
$memAvailable = [math]::Round($os.FreePhysicalMemory / 1MB, 2)

# write stats to host
Write-Host "Stats for $server" -ForegroundColor Green
Write-Host ('-' * 25)
Write-Host "Total Memory      : $memTotal GB"
Write-Host "Available Memory  : $memAvailable GB"
Write-Host "Used Memory       : $($memTotal - $memAvailable) GB"
Write-Host "Operating System  : $($os.Caption)"
Write-Host "System Drive      : $($os.SystemDrive)\"

```
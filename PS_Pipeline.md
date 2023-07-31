# PowerShell Pipeline

## How The Pipeline Works
```PowerShell
# an example (top 5 processes comsuming CPU)
Get-Process |
    Where-Object CPU -gt 0 |
        Sort-Object WorkingSet - Descending |
            Select-Object -First 5

# another example
Get-Process -Nam notepad | Stop-Process

# stop-process parameter help
Get-Help Stop-Process -Parameter *

# byvalue
Get-Process -Name notepad | Stop-Process

# bypropertyname
$np = [pscutomobject]@{name='notepad'}
```
## Select-Object
```PowerShell
# view help topic for Select-Object
Get-Help Select-Object

# store path in variable
$dir = "C:\Users\andre\Test PowerShell"

# view get-childitem members
Get-ChildItem -Path $dir -File | Get-Member

# return specific properties
Get-ChildItem -Path $dir -File -Include "*.png" |
    Select-Object -Property Name, CreationTime, LastWriteTime, LastWriteTime, LastAccessTime, Length |
        Format-Table -AutoSize

# calculated property
Get-ChildItem -Path $dir -File -Include "*.png" |
    Select-Object -Property Name, CreationTime, LastWriteTime, @{name='Length';expression={[math]::round($_.Lenth / 1KB, 2)}} |
        Format-Table -AutoSize

# calculated properties (command in expression)
Get-Content .\servers.txt |
    Select-Object @{n="Server";e={$_}}, @{n="Online",e={(Test-Connection $_ -Quite -Count 1)}}

# return the 5 newest files (sort-object)
Get-ChildItem -Path $dir -File |
    Sort-Object -Property CreationTime -Descending |
        Select-Object -First 5

# view total size of .jpg files (measure-object)
Get-ChildItem -Path $dir -File -Includ "*.jpg" |
    Measure-Object -Property Length -Sum |
        Select-Object sum, @{name='sumMB';expression={[math]::round($_.sum / 1MB, 2)}}

# view number of files by extension (group-object)
Get-ChildItem -Path $dir -File |
    Group-Object -Property extension |
        Sort-Object -Property count -Descending |
            Select-Object -Property Name, Count
```
## Where-Object
## Pipeline Chain Operators
## Pipeline Challenges
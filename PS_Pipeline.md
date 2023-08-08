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

```PowerShell
# view help topic for Where-Object 
Get-Help Where-Object

# store path in variable
$dir = "C:\Users\andre\OneDrive\Pictures"

# basic examples
Get-ChildItem -Path $dir -File | Where-Object -Property Extension -eq ".png"
Get-ChildItem -Path $dir -File | Where-Object Name -like "*pg*"
Get-ChildItem -Path $dir -File | Where-Object -FilterScript {$_.Extension -eq ".jpg" -and $_.Name -like "*tee*"}
Get-ChildItem -Path $dir -File | ? {$_.Extension -eq ".jpg" -and $_.Name -like "*tee*"}

# all files bigger than 500kb
Get-ChildItem -Path $dir -File |
    Where-Object -Property Length -gt 500KB |
        Sort-Object -Property Length -Descending |
            Select-Object Name, Length

# jpg files create today
Get-ChildItem -Path $dir -File |
    Where-Object -FilterScript {$_.Extension -eq ".png" -and $_.LastWriteTime -gt [datetime]::Today} |
        Select-Object Name, LastWriteTime, CreationTime, Length | 
            Sort-Object LastWriteTime -Descending | 
                Format-Table -AutoSize

# make 10 dummy files
foreach ($item in 1..10) {
    New-Item -Path "$dir$((Get-Random 100000).ToString()).png"
}
```
## Pipeline Chain Operators
```PowerShell
# help topic
Get-Help about_Pipeline_Chain_Operators

# simple example
Write-Output "Left" && Write-Output "Right" # if left succeeds, execute right
Write-Output "Left" || Write-Output "Right" # if left fails, execute right

# without pipeline chain operators
$path= ".\servers.txt"

If (-Not (Test-Path -Path $path)) {
    New-Item -Path $path -ItemType File
    @('DC-NUG', 'FS-NUG', 'SQL-NUG') | Out-File -Path $path
}

# with pipeline chain operator
Get-Item -Path $path || New-Item -Path $path -ItemType File

# all the operators!
Get-Item -Path $path ||
    New-Item -Path $path -ItemType File &&
        @('DC-NUG', 'FS-NUG', 'SQL-NUG') | 
            Out-File -Path $path
```
## Pipeline Challenges
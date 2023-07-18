# PowerShell CBT Nugget Notes

- $PSVersionTable
- start pwsh
- PS ISE doesn't work with PS Core, so doesn't work with PS7

# 6. Hello, PowerShell
Get-ComputerType.ps1
```PowerShell
$computers = Get-ADComputer -Filter * #Don't execute at work

foreach ($computer in $computers) {
    
    $os = Get-CimInstance -ClassName Win32_OperatingSystem -ComputerName $c.Name

    $os6

    switch ($os.ProductType) {
        1 { $type = 'Workstation' }
        2 { $type = 'Domain Controller' }
        3 { $type = 'Server' } 
        Default { $type = 'Unkown' }
    }

    "($computer.Name) is a $type"
}
```

``` Powershell
$myArray = "Name1", "Name2", "Name3"

$convertedArray = $myArray | Select-Object @{Name="Name"; Expression={$_.ToString()}}

$convertedArray | Export-Csv -Path "output.csv" -NoTypeInformation
```

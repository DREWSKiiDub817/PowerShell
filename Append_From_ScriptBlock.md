```Powershell
$List = @()

$ComputerNames = @("Billy","Bob","Thorton")

Foreach($name in $ComputerNames){
  $Results = Invoke-Command $name -ScriptBlock{
    $pc = $using:$name

    New-Object -TypeName psobject -Property @{
      'ComputerName' = $pc
    }
  }
  Write-Host $Result.ComputerName -ForegroundColor Green
  $List += $Result.ComputerName
}

$List
```

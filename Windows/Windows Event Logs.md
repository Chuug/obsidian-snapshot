
**Logs location (.evt, .evtx)**
```text
C:\Windows\System32\winevt\Logs
```


#### Get-WinEvent

**Listing of the logs
```PowerShell
Get-WinEvent -ListLog *
```

**Listing of the providers**
```Powershell
Get-WinEvent -ListProvider *
```

**Count output**
```PowerShell
Get-WinEvent -ListProvider * | Measure-Object
```

**List field names**
```powershell
Get-WinEvent -Path .\[.evtx] -MaxEvents 1 | Get-Member
```

**Use a Method listed in Get-Member list field names**
```powershell
(Get-WinEvent -Path .\[.evtx] -MaxEvents 1).GetHashCode()
```

**Select First/Last 10 Object**
```powershell
Get-WinEvent -Path .\[.evtx] | Select-Object * -Last 10
```

**Expand Properties**
```powershell
Get-WinEvent -Path .\[.evtx] -MaxEvent 1 | Select-Object -ExpandProperty Properties
```

**Select all event by id 400 a display all the datas**
```powershell
 Get-WinEvent -Path .\Desktop\[.evtx] | Where-Object { $_.Id -eq 400 } | Select-Object *
 ```
 
**Find 'net1.exe' in logs and display everything**
```powershell
Get-WinEvent -Path .\Desktop\[.evtx] | Where-Object { $_.Message -like '*net1.exe*' } | Select-Object *
```

#### XPath

```PowerShell
Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"]'
```
![[Screenshot_2024-07-11_14-42-01.png]]

**Still working with Sam as the user, what time was Event ID 4724 recorded? (MM/DD/YYYY H:MM:SS [AM/PM]**)
```PowerShell
Get-WinEvent -LogName Security -FilterXPath '*/System/EventID=4724 and */EventData/Data[@Name="TargetUserName"]="sam"'
```

![[Screenshot_2024-07-11_15-04-17.png]]



#### Searching Script
**Execution example**
```powershell
.\search.ps1 -Path .\Hunting_Metasploit_1609814643558.evtx -value 5644 -System
```

<center>search.ps1</center>
```powershell
param (
   [string]$Path,
   [string]$field,
   [string]$value,
   [switch]$System
)

$events = Get-WinEvent -Path $Path
$matchedEvents = @()

if($null -ne $field -and $field -ne '') {
   foreach ($event in $events) {
      $xml = [xml]$event.ToXml()
      foreach ($data in $xml.Event.EventData.Data) {
         if ($data.Name -eq $field -and $data.'#text' -like "*$value*") {
            $matchedEvents += $event
            break
         }
      }
   }
} else {
   foreach ($event in $events) {
      $xml = [xml]$event.ToXml()
      foreach ($data in $xml.Event.EventData.Data) {
         if ($data.'#text' -like "*$value*") {
            $matchedEvents += $event
            break
         }
      }    
   }
}

function Display {
   $match = $args[0]
   foreach ($e in $match) {
      $xml = [xml]$e.ToXml()
      $fields = @()
      "=================="
      "=      Data      ="
      "=================="
      foreach ($data in $xml.Event.EventData.Data) {
         $field = New-Object psobject -Property @{
            Value = $data.'#text'
            Name = $data.Name
         }
         $fields += $field
      }
      $fields | Format-Table -AutoSize

      if($System) {
         "=================="
         "=     System     ="
         "=================="
         foreach ($data in $xml.Event.System) {
            $data
         }
      }
   }
}

Display $matchedEvents
```

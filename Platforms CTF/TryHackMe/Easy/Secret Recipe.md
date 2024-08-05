#windows #registry

https://tryhackme.com/r/room/registry4n6

---

**Q1. What is the Computer Name of the Machine found in the registry?**

```text
SYSTEM\ControlSet001\Control\ComputerName\ComputerName
```
The location is in, and the answer is **JAMES**

**Q2. When was the Administrator account created on this machine?**

```text
SAM\SAM\Domains\Account\Users\Names\Administrator
```
The location of the Administrator account is in, the answer is the Last write timestamp: **2021-03-17 14:58:48**

**Q3. What is the RID associated with the Administrator account?**

The default RID for the Administrator is 500 but we can calculate the hex of `0x1F4` in decimal, which is `500`

**Q4. How many User accounts were observed on this machine?**

```text
SAM\SAM\Domains\Account\Users\Names\
```
There are **7** names in 

**Q5. There seems to be a suspicious account created as a backdoor with RID 1013. What is the Account Name?**

**bdoor** seems to be the account be we can confirm it by calculating `0x3F5` equal to `1013` in decimal

**Q6. What is the VPN connection this host connected to?**

```text
NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count
``` 
I went in to see softwares. There are many `{GUID}` in `UserAssist` and I needed to open them all to find the one with all the `.exe`

I found the **ProtonVPN** executable

**Q7. When was the first VPN connection observed?**

```text
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList
```
We can see the network list in, the response in the `First Connect LOCAL` column, which is **2022-10-12 19:52:36**

**Q8. There were three shared folders observed on his machine. What is the path of the third share?**

```text
SYSTEM\ControlSet001\Services\LanmanServer\Shares
```

The third path is **C:\RESTRICTED FILES**

**Q9. What is the Last DHCP IP assigned to this host?**

```text
SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\[GUID]
```
I found network interface informations there and I had the DHCP information, which was **172.31.2.197** for the `DhcpIPAddress` value

**Q10. The suspect seems to have accessed a file containing the secret coffee recipe. What is the name of the file?**

```text
NTUSER.DAT\Software\Microsoft\Windows \CurrentVersion\Explorer\RecentDocs
```

There are all the information needed about the recent files, the answer is **secret-recipe.pdf**

**Q11. The suspect ran multiple commands in the run windows. What command was run to enumerate the network interfaces?**

```text
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
```

There I found the command **pnputil /enum-interfaces**

**Q12. In the file explorer, the user searched for a network utility to transfer files. What is the name of that tool?**

```text
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

The tool is **netcat**

**Q13. What is the recent text file opened by the suspect?**

```text
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

The answer is **secret-code.txt**

**Q14. How many times was Powershell executed on this host?**

```text
NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count
``` 

The `Run Counter` of `powershell.exe` is **3**

**Q15. The suspect also executed a network monitoring tool. What is the name of the tool?**

```text
NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count
``` 

**wireshark** seems to be the monitoring tool

**Q16. Registry Hives also notes the amount of time a process is in focus. Examine the Hives. For how many seconds was ProtonVPN executed?**

```text
NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count
``` 

`ProtonVPN.exe` has a `Focus Time` of 05m, 43s, so **343** seconds

**Q17. Everything.exe is a utility used to search for files in a Windows machine. What is the full path from which everything.exe was executed?**

```text
NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count
``` 

The **C:\Users\Administrator\Downloads\tools\Everything\Everything.exe** path is there


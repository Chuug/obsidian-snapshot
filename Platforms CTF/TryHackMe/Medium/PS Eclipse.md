https://tryhackme.com/r/room/posheclipse

---
**Q1**
```text
index="main" .exe | dedup Image
```

Found the suspicious `C:\Windows\Temp\OUTSTANDING_GUTTER.exe` 

**Q2**
```text
index="main" *ngrok*
```

The url is in the `QueryName` but it is the Q6 

**Q3**
It is obviously `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` 

**Q4**
```text
index="main" powershell OUTSTANDING_GUTTER.exe | table CommandLine | dedup CommandLine
```
Filter by top CommandLine

**Q5**
```text
index="main" powershell "OUTSTANDING_GUTTER.exe" | table CommandLine | dedup CommandLine
```

`NT AUTHORITY\SYSTEM` is the user

**Q6**
```text
index="main" OUTSTANDING_GUTTER.exe EventCode=22
```

The url is the `QueryName` field

**Q7**
```text
index="main" .ps1
```

**Q8**
```text
index="main" script.ps1
```
Put the MD5 on virus total, the script original name is `BlackSun.ps1`

**Q9**
```text
index="main" BlackSun
```

**Q10**
```text
index="main" BlackSun
```
https://tryhackme.com/r/room/newhireoldartifacts

---

`11111.exe` is the first binary, `IonicLarge.exe` seems to be the second, need the third ?

**Q1**
```text
index="main" EventCode=1 .exe MD5 | dedup Image
```
Put MD5 on virus total until I found the `Web Browser Pass View` as `11111.exe`

**Q2**
```text
index="main" 11111.exe | table Company | dedup Company
```

**Q3**
```text
index="main" Image="*\\AppData\\Local\\Temp\\*" | dedup Image | table Image
```
Because `\FINANC~1\` can be written like `\Finance01\`, I had to use `*\\AppData\\Local\\Temp\\*` to discover `IonicLarge.exe

```text
index="main" IonicLarge.exe MD5
```
I found the original name by putting all the MD5 on virtustotal and I finally got `PalitExplorer.exe`

**Q4**
```text
index="main" IonicLarge.exe 
```
Clicking on `Destination Ip` on the left pane shows the ip with the 2 outbound connections: `2[.]56[.]59[.]42`

**Q5**

```text
index="main" IonicLarge.exe (EventCode=11 OR EventCode=12 OR EventCode=13) | table TargetObject | dedup TargetObject
```
EventCode 11, 12, 13 are for windows registry and it clearly shows up `HKLM\SOFTWARE\Policies\Microsoft\Windows Defender` multiple times

**Q6**
```text
index="main" "taskkill /im"
```
The hint gave me `taskkill /im` which is a good way to find killed process or I could use `EventCode=5` but there was much more results

**Q7**
```text
index="main" PowerShell | dedup CommandLine | table CommandLine
```
The second top line was the last powershell command executed


**Q8**
```text
index="main" "ThreatIDDefaultAction_Ids=" | dedup CommandLine | table CommandLine
```
Just put the IDs in the order

**Q9**
```text
index="main" Image="*\\AppData\\*.exe" MD5 | dedup Image | table Image, Hashes
```
One by one, put the MD5s on virus total on until it matches the correct response

**Q10**
```text
index="main" EasyCalc.exe EventCode=7  | dedup OriginalFileName | table OriginalFileName
```
EventCode 7 is for image loading, the 3 dlls were the 3 firsts on the list


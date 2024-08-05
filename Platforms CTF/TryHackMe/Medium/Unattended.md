#forensic #windows 

https://tryhackme.com/r/room/unattended

---

**Q1 & Q2**

I had to import the THM-RFedora's `NTUSER.DAT`

```text
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

Both response **.pdf** and **continental** were there

**Q3 & Q4**

Both were in the `Web Download` category not present on my own Windows, only on the room's machine

The responses are **7z2201-x64.exe** and **2022-11-19 12:09:19 UTC**

**Q5**

```text
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

The `continental.png` were open on **2022-11-19 12:10:21**

**Q6**

I bruteforced the response because fuck it, it's **2**

**Q7**

```text
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

The response was in the `Opened On` column for `launchcode.txt`

**Q8 & Q9**

Both in the `Web History` category on Autopsy

The response are **hxxps://pastebin.com/1FQASAav** and **ne7AIRhi3PdESy9RnOrN**

https://tryhackme.com/r/room/investigatingwindows

---
```shell
xfreerdp /u:Administrator /p:letmein123! /v:10.10.177.58 /dynamic-resolution
```

I found john's last log on by typing this on cmd
```powershell
net user john
```

Event viewer has a filter
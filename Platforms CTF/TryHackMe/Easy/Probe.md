#enumeration #scanning #wpscan 

https://tryhackme.com/r/room/probe

---
Firstly I scanned all the port with
```shell
nmap -p- 10.10.204.0
```
The I scanned the discovered port more aggressively
```shell
nmap -sV -A -p22,80,443,1338,1443,1883,8000,9007 10.10.204.0
```

I got the Apache server `2.4.41` and the ftp port `1338`

I've got the ftp welcome message by typing `ftp 10.10.204.0 1338`

Found a page on https://10.10.204.0 and the certificate made on dev.probe.thm

Found the wordpress on https//10.10.204.0:9007, changed the /etc/hosts to myblog.thm to fit the inblog's links

I've scanned the worpress by disabling the tls check
```shell
wpscan --url https://myblog.thm:9007 --disable-tls-checks
```


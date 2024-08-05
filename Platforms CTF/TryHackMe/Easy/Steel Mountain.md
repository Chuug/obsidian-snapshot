https://tryhackme.com/r/room/steelmountain

---
#### Nmap
![[Screenshot_2024-05-11_20-33-40.png]]

The employee of the month was on `10.10.54.149:80`

There another http port on 8080, it's an ftp server called HFS

I had to use the CVE `2014-6287` on metasploit to gain user access.

Then I downloaded something for the privilege escalation [here](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1)

![[Screenshot_2024-05-11_22-03-58.png]]


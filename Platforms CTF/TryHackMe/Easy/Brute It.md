
https://tryhackme.com/r/room/bruteit

---
#### Nmap
![[Screenshot_2024-06-09_17-01-24.png]]

I found the secret page `/admin` with gobuster and a note in html commentary tells to a `john` that the username is `admin` 

I used hydra to bruteforce the login
```shell
hydra -l admin -P /usr/share/wordlists/rockyou.txt 'http://10.10.137.206/admin/index.php:user=^USER^&pass=^PASS^:F=Username or password invalid'
```

And the creds are `admin:xavier`

In the admin panel, I found the private ssh key that I cracked with john and it gave me john's creds: `john:rockinroll`

And here I am in.

The `sudo -l` gave me `cat` so gg ez
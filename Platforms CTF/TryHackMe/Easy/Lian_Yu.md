#pkexec

https://tryhackme.com/r/room/lianyu

---
#### Nmap
![[Screenshot_2024-05-25_17-01-45.png]]

I found the `/island` path with `gobuster` 

There is the `Lian_Yu` username with a "Code Word" as `vigilante`

The `/island` path have a `/2100` path in it

We are `http://10.10.139.128/island/2100/` so far

Then I got an hint, needed to search for a .ticker with `gobuster` again and here I m on `http://10.10.139.128/island/2100/green_arrow.ticket`
![[Screenshot_2024-05-25_17-23-16.png]]
It was a `base 58` encoded password, which gave me `!#th3h00d` 

So I used these creds `vigilante:!#th3h00d` to connect on the ftp:
```shell
ftp 10.10.139.128
```

I found 3 images, the `.jpg` had 2 text files hidden, the password for extract these files was hidden in `Leave_me_alone.png` and it was `password`
And I've go the `Slade Wilson` username in a hidden

In the `shado` file, there is a password (thx thm) `M3tahuman`

I used these creds to login through ssh `slade:M3tahuman` and here i m in

The `sudo -l` gave me `pkexec`:
```shell
sudo pkexec /bin/bash
cat /root/root.txt
```

And here I m root
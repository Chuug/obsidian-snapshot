
https://tryhackme.com/r/room/lazyadmin

---
#### Nmap
![[Screenshot_2024-05-16_11-41-59.png]]

I've found the `/content` by enumerating the port `80`. I forgot to disable the foxy proxy so I have this fucking 
![[Screenshot_2024-05-16_11-50-21.png]]

`/content/`'s enumeration:
![[Screenshot_2024-05-16_11-58-06.png]]

The `changelog.txt` is telling changlog about `basic-cms`

I found a `sql` dump file with a hashed password in
`42f749ade7f9e195bf475f37a44cafcb`

```shell
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash
```

Found this :![[Screenshot_2024-05-16_13-14-18.png]]

The user was in the `sql` dump aswell: `manager:Password123`

I found a place were I can create a page and upload file + extract zip at `http://10.10.79.142/content/as/?type=post`. So, I got a reverse shell and here I m in
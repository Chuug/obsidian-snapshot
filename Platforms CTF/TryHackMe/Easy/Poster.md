
https://tryhackme.com/r/room/poster

---
#### Nmap
![[Screenshot_2024-06-08_15-05-00.png]]

I followed the semi-walktrough (freepoint lul) until I got in.

I found `dark`'s creds on his home:
`dark:qwerty1234#!hackme`

And here I am in as `dark`

I've found alison's password in the `config.php` in `/var/www/html` and I used it to connect through ssh via her creds `alison:p4ssw0rdS3cur3!#`

And here I am in as `alison` with the `user.txt`

She is in the sudo group, easy win
```shell
sudo cat /root/root.txt
```


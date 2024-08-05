#basic-auth #hydra #cadaver

https://tryhackme.com/r/room/bsidesgtdav

---
#### Nmap
![[Screenshot_2024-05-14_22-01-40.png]]
Ok I found `/webdav` with gobuster and it is a `basic-auth` and I think the username is `dav` so let's use hydra on this:
```shell
hydra -l dav -P /usr/share/wordlists/rockyou.txt -f [target-ip] http-get /webdav
```

Hydra didn't worked so I googled webdav and I realized that was a software, not a person, here is some interresting [informations](https://xforeveryman.blogspot.com/2012/01/helper-webdav-xampp-173-default.html?m=1) 

So I used `cadaver` like suggested
```shell
cadaver http://[target-ip]/webdav
```

I put `wampp:xampp` as credentials and I was connected and able to put a reverse shell:
```shell
put /path/to/my/revshell
```

And here I m in, the `sudo -l` gave me `/bin/cat /root/root.txt` and the `user.txt` was in `merlin`'s home
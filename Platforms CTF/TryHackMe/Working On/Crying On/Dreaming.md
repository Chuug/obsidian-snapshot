https://tryhackme.com/r/room/dreaming

```shell
gobuster dir -u http://10.10.38.253 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 200
```

/app/pluck-4.7.13

In /app/pluck-4.7.13
	/images
	/data
		/themes
		/modules
		/image
		/settings
		/inc
			/lib
			/lang
		/trash
	/files
	/docs
	/login.php

password test: dreaming, sandman, heaven, kingdom


CVE:  https://www.exploit-db.com/exploits/49909


#### ffuf


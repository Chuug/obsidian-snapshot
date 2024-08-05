#borg

https://tryhackme.com/r/room/cyborgt8

---
#### Nmap
![[Screenshot_2024-06-02_10-53-39.png]]

I found `/etc` and `/admin` with gobuster. 

In the `/admin/admin.html`, there are notes left by people named `Josh`, `Adam` and `Alex`  and there is a `archive.tar` file to download

In `/etc` there is a file `passwd` with credentials I have cracked with hashcat
```shell
hashcat -a 0 -m 1600 -O hash /usr/share/wordlists/rockyou.txt
```

And those creds are `music_archive:squidward`

There is also a `squid.conf` in `/etc`

The `README` in `archive.tar` bring me to [Borg](https://borgbackup.readthedocs.io/en/stable/)

When I am in the `/final_archive`, I am able to list borg's repo
```shell
borg list ./
```

![[Screenshot_2024-06-15_16-09-23.png]]
Then I just have to extract `music_archive` with the password above
```shell
borg extract ./::music_archive
```

It give me `alex` entire home directory with a `note.txt` containing his ssh password, so here are those creds: `alex:S3cretP@s3` 

And here I m in as `alex`

The `sudo -l` gave me the `/etc/mp3backups/backup.sh` script with the following interresting part
![[Screenshot_2024-06-15_17-12-36.png]]

This means I can execute a command as sudo with the `-c` flag
```shell
sudo /etc/mp3backups/backup.sh -c "cat /root/root.txt"
```

And here is the root flag

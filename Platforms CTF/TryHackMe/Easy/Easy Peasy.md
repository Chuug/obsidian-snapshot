
https://tryhackme.com/r/room/easypeasyctf

---
#### Nmap
![[Screenshot_2024-05-27_11-33-27.png]]

So far, I've got `/hidden/whatever` on port `80` and a probable `md5` hash in the `robots.txt` on port `65524`:
```text
User-Agent:*
Disallow:/
Robots Not Allowed
User-Agent:a18672860d0510e5ab6699730763b250
Allow:/
This Flag Can Enter But Only This Flag No More Exceptions
```

I used this [website](https://md5hashing.net/) to crack the hash

I saved I default `debian 2.4` page to use it for `diff` with the one in this ctf and I've go 2 main changes:
```text
<p hidden>its encoded with ba....:ObsJmP173N2X6dOrAgEAL0Vu</p>
```

```text
Fl4g 3 : flag{9fdafbd64c47471a8f54cd3fc64cd312}
```

The flag 3 cracked is `candeger`, probably the user

For the encoded text, I used cyberchef to `base62` decode:
`/n0th1ng3ls3m4tt3r`

This hidden page gave me `940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81`

There was also an image, it was the image the solution, not the above hash
I crack the password of `binarycodepixabay.jpg` with the password list gave by the CTF at the beginning:
```shell
stegseek binarycodepixabay.jpg easypeasy.txt
```

The steg password is `mypasswordforthatjob`:
```shell
steghide extract -sf binarycodepixabay.jpg
```

This gave me the username `boring` and a password encoded in binary, so the creds are `boring:iconvertedmypasswordtobinary`

And here I m in

The user flag was in `ROT13`

For the root, there is simply a crontab job running a bash script in sudo and I can edit this script so easy win
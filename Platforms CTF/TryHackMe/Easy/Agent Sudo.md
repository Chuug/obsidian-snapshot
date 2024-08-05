#steghide #hydra #ftp 

https://tryhackme.com/r/room/agentsudoctf

---
#### Nmap
![[Screenshot_2024-05-15_18-34-47.png]]
I juste enumerated `/icons/small` as forbidden directories for now
The first message
```markdown
Dear agents,  
  
Use your own **codename** as user-agent to access the site.  
  
From,  
Agent R
```

With the user-agent C on `/agent_C_attention.php`
```markdown
Attention chris,  
  
Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!  
  
From,  
Agent R
```

There are some agents:
R
Chris
James

Maybe I can brute force the ftp now:
```shell
hydra -l chris -P /usr/share/wordlists/rockyou.txt 10.10.192.72 ftp
```

![[Screenshot_2024-05-15_19-15-54.png]]

Ok I download one `.txt` and 2 images from the ftp:
```markdown
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.

From,
Agent C
```

So I downloaded a new `steghide` cracker called [stegseek](https://github.com/RickdeJager/stegseek) to replace the old `stegcracker`:
```shell
stegseek cute-alien.jpg /usr/share/wordlists/rockyou.txt
```
And the new message is:
```markdown
Hi james,

Glad you find this message. Your login password is hackerrules!

Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris
```
Here is `James`, a new name with a password `hackerrules!`

I used the [CVE-2019-14287](https://www.exploit-db.com/exploits/47502) exploit to easily privesc to root


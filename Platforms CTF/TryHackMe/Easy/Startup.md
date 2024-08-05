
https://tryhackme.com/r/room/startup

---
#### Nmap
![[Screenshot_2024-06-01_18-43-04.png]]

I first download all the ftp with
```shell
wget -m ftp://anonymous@10.10.1.180
```

There is a `notice.txt` ~~with a possible username `Maya`~~:
```text
Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our w  
ebsite will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus.
```
<small>The note was telling me we can upload on the ftp</small>

And an image `important.jpg`

I can write in the ftp with
```shell
put [file]
```

So I put a reverse shell and here I am in

I found the cute note `/recipe.txt` with the ingredient: love, so cute

Then I found a `suspicious.pcapng` in `/incidents`, I'll get it by setting up a python server

I used `CTRL+F` on wireshark to find something related to a password:
![[Screenshot_2024-06-01_19-20-57.png]]
The password was nearby and it is lennie's

So the creds are `lennie:c4ntg3t3n0ughsp1c3`

And here I m in as Lennie

The lennie's script `/etc/print.sh` is ran by root every minute so I changed it
```shell
#!/bin/bash
cat /root/root.txt > /tmp/flag
```


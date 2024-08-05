#git 

https://tryhackme.com/r/room/githappens

---
#### Nmap
![[Screenshot_2024-05-27_15-07-33.png]]
So there was a `/.git` repo on the port `80`

I downloaded the repo with
```shell
wget -r -np http://10.10.156.232/.git
```

Then I checked the logs with `git log`

I just had to go backward though commits and find a way to the super password and it was this one:
![[Screenshot_2024-05-27_17-20-28.png]]


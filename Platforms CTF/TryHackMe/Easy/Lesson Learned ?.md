https://tryhackme.com/r/room/lessonlearned

[Actual doc](https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/)

I have to brute force the login page using hydra
![[Screenshot_2024-04-29_20-25-32.png]]I found multiple accounts, now it's time to bruteforce some user's password

```shell
sudo hydra -l "martin" -P /usr/share/wordlists/rockyou.txt 10.10.73.243 http-post-form "/:username=^USER^&password=^PASS^:Invalid password."
```

Nothing happened, it's something else

It was sql injection
`karen' and 1=1;-- -`

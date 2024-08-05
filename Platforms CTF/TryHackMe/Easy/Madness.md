
https://tryhackme.com/r/room/madness

---
#### Nmap 
![[Screenshot_2024-05-22_11-59-24.png]]

There was a `password.txt` in the image on THM's page containing the `*axA&GF8dP` password

There was an image on the `Apache2` default page named `thm.jpg`, it is corrupted. It was a fucking JFIF, I replaced the first bytes by `FF D8 FF E0 00 10 4A 46 49 46 00 01` leading me to the `/th1s_1s_h1dd3n` page

I am landing on a page asking me for "a secret", the html comment gives me a hint
![[Screenshot_2024-05-22_13-09-00.png]]
The page name is called `index.php`, so it will probably be a POST request

It was a `GET` request and I used `ffuf` to discover the right number:
```shell
ffuf -u 'http://10.10.119.195/th1s_1s_h1dd3n/?secret=FUZZ' -w numbers.txt -fw 45
```

I populated `numbers.txt` with:
```shell
seq 0 99 > numbers.txt
```

The answer gave me this `y2RPJ4QaPF!B`. I used this to
```shell
steghide extract -sf thm.jpg
```
And I've got `hidden.txt` containing `wbxre` username. I used ROT cipher to get the real username `joker`

So here are the creds `joker:*axA&GF8dP`

```shell
ssh joker@10.10.119.195
```

I found an exploit for the `SUID screen-4.5.0` on [exploit-db](https://www.exploit-db.com/exploits/41154)
I just ran the bash script and here I am root


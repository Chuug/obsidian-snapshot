https://tryhackme.com/r/room/snortchallenges2

---

#### Scenario 1

The port was obviously 80 or 22 and it was 22 for ssh so I made a drop rule to block everything from

Firstly, I made a rule to filter the port 22 request
```text
alert tcp any any <> any 22 (msg:"alert";sid:1)
```

Then I identified the `10.10.245.36` ip and made the drop rule

The drop rule
```text
drop tcp 10.10.245.36 any <> 10.10.140.29 22 (msg:"get fucked";sid:1)
```

```shell
sudo snort -c rule2 -q -Q --daq afpacket -i eth0:eth1 -A full
```

#### Scenario 2

Firstly I start to monitor
```shell
sudo snort -X
```

In the console, I found outgoind traffic to `10.10.144.156:4444`, used by `metasploit` and I just had to make the rule

```text
drop tcp any any -> 10.10.144.156 4444 (msg:"get fucked";sid:1)
```

```shell
sudo snort -c rule -q -Q --daq afpacket -i eth0:eth1 -A full
```

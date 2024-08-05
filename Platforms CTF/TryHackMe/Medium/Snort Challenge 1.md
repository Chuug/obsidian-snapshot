https://tryhackme.com/r/room/snortchallenges1

---

#### Writing IDS Rules (HTTP)

**Q1**
```text
alert tcp any any <> any 80 (msg:"alert";sid:1)
```

**`164`** alerts

**Q2 - 7**
I can read the 63th alert with
```shell
awk 'BEGIN {RS="\n\n"} NR==63' alert
```

#### Writing IDS Rules (FTP)

**Q1**
```text
alert tcp any any <> any 21 (msg:"ftp";sid:1)
```

**`307`** alerts

**Q2**
```shell
snort -r [snort-log-file] -d 'port 21' -n 20
```

**Q3**

First way
```shell
snort -r [snort-log-file] -d 'port 21' > filter.txt
grep '530 User' filter.txt | wc -l
```

Rule way
<center>local.rules</center>
```text
alert tcp any any <> any 21 (msg:"alert";content:"530 User";sid:1)
```

```shell
snort -c local.rules -r [.pcap]
```

Displays **`41`** alerts

**Q4**
<center>local.rules</center>
```text
alert tcp any any <> any 21 (msg:"alert";content:"230 User";sid:1)
```

**Q5**
<center>local.rules</center>
```text
alert tcp any any <> any 21 (msg:"alert";content:"331 ";sid:1)
```


**Q6**
<center>local.rules</center>
```text
alert tcp any any <> any 21 (msg:"alert";content:"331 Password required for Administrator";sid:1)
```


#### Writing IDS Rules (PNG)
**Q1**
<center>local.rules</center>
```text
alert tcp any any -> any any (msg:"alter"; content:"|89504E470D0A1A0A|"; sid:1)
```
It is the png magic byte


**Q2**
<center>local.rules</center>
```text
alert tcp any any -> any any (msg:"alter"; content:"|47494638|"; sid:1)
```
The first magic bytes of gifs

#### Writing IDS Rules (Torrent Metafile)

**Q1-4**
<center>local.rules</center>
```text
alert tcp any any <> any any (msg:"alert";content:".torrent";sid:1)
```

```shell
sudo snort -c local.rules -r torrent.pcap -l .
sudo snort -r snort.log -d > dump.txt
cat dump.txt
```

The responses are in the `dump.txt`

#### Troubleshooting Rule Syntax Errors

**Q1**
```text
alert tcp any 3372 -> any any (msg:"Troubleshooting 1";sid:1)
```

**Q2**
```text
alert icmp any any -> any any (msg:"Troubleshooting 2";sid:1)
```

**Q3**
```text
alert icmp any any -> any any (msg:"ICMP Packet Found";sid:1)
alert tcp any any -> any 80,443 (msg:"HTTPX Packet Found";sid:2)
```

**Q4**
```text
alert icmp any any -> any any (msg:"ICMP Packet Found";sid:1)
alert tcp any 80,443 -> any any (msg:"HTTPX Packet Found";sid:2)
```

**Q5**
```text
alert icmp any any <> any any (msg:"ICMP Packet Found";sid:1)
alert icmp any any <> any any (msg:"Inbound ICMP Packet Found";sid:2)
alert tcp any any -> any 80,443 (msg:"HTTPX Packet Found";sid:3)
```

**Q6**
```text
alert tcp any any <> any 80 (msg:"GET Request Found";content:"GET";sid:1)
# OR
alert tcp any any <> any 80 (msg:"GET Request Found";content:"|47 45 54|";sid:1)
```

**Q7**
**`msg`** is required in the rules

#### Using External Rules (MS17-010)

**Q1**
```shell
sudo snort -c local.rules -r ms-17-010.pcap 
```

**`25154`** outputs

**Q2**
```text
alert tcp any any <> any any (msg:"alert";content:"\\IPC$";sid:1)
```

```shell
sudo snort -c local.rules -r ms-17-010.pcap -l .
```

**Q3**
```shell
sudo snort -r snort.log -d > dump.txt
cat dump.txt
```

**Q4**
https://www.cvedetails.com/cve/CVE-2017-0144/

#### Using External Rules (Log4j)

**Q1-3**
```shell
sudo snort -c local.rules -r log4j.pcap -l .
```
Responses are in the report

**Q4**
New rules
```text
alert tcp any any <> any any (msg:"alert";dsize:770<>855;sid:1)
```

```shell
sudo snort -c local.rules -r log4j.pcap -l .
```

**Q5-6**
The rule for target the specific paquet
```text
alert tcp any any <> any any (msg:"alert";id:62808;sid:1)
```

```shell
sudo snort -r snort.log -d > dump.txt # snort.log from Q4
cat dump.txt
```

**Q7**
The base 64 is in the paquet from the rule

**Q8**
https://nvd.nist.gov/vuln/detail/CVE-2021-44228


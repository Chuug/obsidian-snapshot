
https://tryhackme.com/r/room/zeekbroexercises

---
#### Anomalous DNS

**Q1**
```shell
cat dns.log  | grep AAAA | wc -l
```

**Q2**
```shell
cat conn.log | zeek-cut duration | sort -nr | head -1 
```

**Q3**
```shell
cat dns.log | zeek-cut id.resp_h | sort | uniq | wc -l #4 +
cat dns.log | zeek-cut id.orig_h | sort | uniq | wc -l #2 = 6
```

**Q4**
```shell
cat conn.log | zeek-cut id.orig_h | sort | uniq | head -1
```

#### Phishing

**Q1**
```shell
cat conn.log | zeek-cut id.orig_h | uniq -c
```

**Q2**
```shell
cat http.log | zeek-cut host uri | grep knr.exe
```

**Q3**
```shell
md5sum extract-1561667899.060086-HTTP-FOghls3WpIjKpvXaEl 
```
Then go to virustotal.com (Q4)

**Q4**
```text
https://www.virustotal.com/gui/file/749e161661290e8a2d190b1a66469744127bc25bf46e5d0c6f2e835f4b92db18
```

**Q5**
```text
https://www.virustotal.com/gui/file/749e161661290e8a2d190b1a66469744127bc25bf46e5d0c6f2e835f4b92db18/relations
```
The domain with the most detection

**Q6**
`knr.exe`

#### Log4J
```shell
zeek -Cr log4shell.pcapng detection-log4j.zeek
```

**Q1**
```shell
cat signatures.log | zeek-cut | wc -l
```

**Q2**
```shell
cat http.log | zeek-cut user_agent
```

**Q3**
```shell
cat http.log | zeek-cut uri | uniq
```

**Q4**
```shell
cat log4j.log | zeek-cut uri | uniq | cut -d '/' -f 5 
```



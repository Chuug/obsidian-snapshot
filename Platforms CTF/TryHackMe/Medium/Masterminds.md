#brim

https://tryhackme.com/r/room/mastermindsxlq

---
### Infection 1
**Q1**
```Zed
cut id.orig_h | sort | uniq -c | sort -r count | fuse
```
I isolated all host ip and sorted by count, the highest were probably the one

**Q2**
```Zed
_path=="http" status_code==404 | cut host
```

**Q3**
```Zed
_path=="http" response_body_len==1309 | cut host, id.resp_h
```

**Q4**
```Zed
_path=="dns" lower(query)=='cab.mykfn.com' | count()
```

**Q5**
```Zed
_path=="http" id.orig_h==192.168.75.249 host=="bhaktivrind.com" | cut uri
```

**Q6**
```Zed
_path=="http" id.orig_h==192.168.75.249 | cut id.resp_h, uri
```

### Infection 2
**Q1**
```Zed
cut id.orig_h | sort | uniq -c | sort -r | fuse
```

**Q2**
```Zed
_path=="http" id.orig_h==192.168.75.146 | method=="POST" | cut host | uniq
```

**Q3**
```Zed
_path=="http" id.orig_h==192.168.75.146 | method=="POST" | cut host | count()
```

**Q4**
```Zed
_path=="http" method=="GET" | cut host
```

**Q5**
```Zed
_path=="http" method=="GET" | cut uri
```

**Q6**
```Zed
_path=="dns" query=="hypercustom.top" | cut answers
```

**Q7**
```Zed
alert.category=="A Network Trojan was detected" | cut src_ip, dest_ip | uniq
```

**Q8**
```text
https://urlhaus.abuse.ch/url/1552560/
```

### Infection 3
**Q1**
```Zed
cut id.orig_h | sort | uniq -c | sort -r | fuse
```

**Q2**
```Zed
_path=="http" id.orig_h==192.168.75.232 | cut host, uri | sort | uniq
```

**Q3**
```Zed
_path=="dns" query=="efhoahegue.ru" OR query=="afhoahegue.ru" OR query=="xfhoahegue.ru" | cut answers | sort | uniq
```

**Q4**
```Zed
_path=="dns" query=="efhoahegue.ru" | count()
```

**Q5**
```Zed
_path=="http" method=="GET" host=="efhoahegue.ru" | where uri ~/.exe/ | count()
```

**Q6**
```Zed
_path=="http" method=="GET" host=="efhoahegue.ru" | where uri ~/.exe/ | cut user_agent | uniq
```

**Q7**
```Zed
_path=="dns" | count()
```

**Q8**
```Zed
https://appriver.com/blog/phorphiextrik-botnet-campaign-leads-to-multiple-infections-ransomware-banking-trojan-cryptojacking
```



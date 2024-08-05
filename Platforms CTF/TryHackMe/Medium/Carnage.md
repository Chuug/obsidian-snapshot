#brim 

https://tryhackme.com/r/room/c2carnage

---
The malicious ip
```text
85.187.128.24
```

Victime host 
```text
10.9.23.102
```

**Q1-3**
```Zed
_path=="http" | cut ts,id.orig_h, id.resp_h, host, uri | sort | uniq -c
```

**Q4**
```Zed
_path=="files" id.resp_h==85.187.128.24 | cut md5
```

Then I go to [virus total](https://www.virustotal.com/gui/file/77229c744a0b1470afc7989a774cfe821386c11c0165e7e3fb5e9897a789a8cb/relations) to see the relations

**Q5-6**
```Wireshark
ip.src== 85.187.128.24 && _ws.col.protocol == "HTTP"
```

**Q7**
```Wireshark
frame.number >= 2422 && frame.number <= 3621 && tls.handshake.type == 1
```

**Q8**
Domain is `finejewels.com.au`
Ip is `148.72.192.206`
Frame `2436`
```Wireshark
(ip.src == 148.72.192.206 || ip.dst == 148.72.192.206) && tls
```

**Q9**
```Zed
cut id.resp_h | sort | uniq -c | sort -r count | fuse
```
VirusTotal exposed `185.125.204.174` & `185.106.96.158`

**Q10**
```Wireshark
ip.src == 185.106.96.158 || ip.dst == 185.106.96.158
```
Search `host` on the subfilter

**Q11**
Found the domain on [VirusTotal](https://www.virustotal.com/gui/ip-address/185.106.96.158/relations)

**Q12**
Found the domain on [VirusTotal](https://www.virustotal.com/gui/ip-address/185.125.204.174/relations)

**Q13**
```Wireshark
http.request.method == "POST"
```
Search `host` on the subfilter

**Q14**
```Wireshark
http.request.method == "POST"
```

**Q15**
```Wireshark
http.request.method == "POST"
```

**Q16**
```Wireshark
ip.src == 208.91.128.6 && http
```
Follow HTTP stream to see the Server header

**Q17-18**
```Wireshark
dns && frame contains "api"
```

**Q19**
```Wireshark
smtp contains "MAIL FROM"
```

**Q20**
```Wireshark
api.ipify.org
```

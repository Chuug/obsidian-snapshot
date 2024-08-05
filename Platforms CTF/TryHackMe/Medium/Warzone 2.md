#brim 

https://tryhackme.com/r/room/warzonetwo

---
**Q1**
```Zed
event_type=="alert" | cut alert.signature | sort | uniq
```

**Q2**
```Zed
"Potential Corporate Privacy Violation" | cut alert.signature
```

**Q3**
```Zed
"Potential Corporate Privacy Violation" OR "A Network Trojan was Detected" | cut src_ip | uniq
```

**Q4**
```Zed
185.118.164.8 _path=="http" | put url := host + uri | cut url
```

**Q5**
Isolate the MD5
```Zed
_path=="files" "gap1.cab" | cut md5
```
Then [VirusTotal](https://www.virustotal.com/gui/file/3769a84dbe7ba74ad7b0b355a864483d3562888a67806082ff094a56ce73bf7e) gave me the `draw.dll`

**Q6**
```Zed
_path=="http" "gap1.cab" | cut user_agent
```

**Q7**
Still in [VirusTotal](https://www.virustotal.com/gui/file/3769a84dbe7ba74ad7b0b355a864483d3562888a67806082ff094a56ce73bf7e/relations) , in Contacted Domains

**Q8**
```Zed
"Not Suspicious Traffic" | cut src_ip | sort | uniq
```

**Q9**
The malicious domains in [VirusTotal](https://www.virustotal.com/gui/ip-address/64.225.65.166/relations) for 64.225.65.166

**Q10**
```Zed
_path=="dns" 142.93.211.176 | cut query
```

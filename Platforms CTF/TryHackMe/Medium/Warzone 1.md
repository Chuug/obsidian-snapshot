#brim #wireshark 

https://tryhackme.com/r/room/warzoneone

---
**Q1**
```Zed
event_type=="alert" | cut alert.signature | sort | uniq
```

**Q2**
```Zed
event_type=="alert" alert.signature=="ET MALWARE MirrorBlast CnC Activity M3" | cut src_ip | uniq
```

**Q3**
```Zed
event_type=="alert" alert.signature=="ET MALWARE MirrorBlast CnC Activity M3" | cut dest_ip | uniq
```

**Q4**
On [VirusTotal](https://www.virustotal.com/gui/ip-address/169.239.128.11/community) we can see the **TA505** group on the community section

**Q5**
Still on [VirusTotal](https://www.virustotal.com/gui/file/6841b26b9218688de6318b083cb70ecdca65876455a1723be00b383844c71f42/detection) (a binary from the link Q4) in the Detection section in familly label

**Q6**
On [VirusTotal](https://www.virustotal.com/gui/ip-address/169.239.128.11/community) , in Relations -> Communicating Files

**Q7**
```Zed
_path=="http" id.resp_h==169.239.128.11 | cut user_agent | uniq
```
**Q8**
Found the responses in this [article](https://threatresearch.ext.hp.com/mirrorblast-and-ta505-examining-similarities-in-tactics-techniques-and-procedures/) on MirrorBlast/TA505 campaign in IOCS section:
185[.]10[.]68[.]235
192[.]36[.]27[.]92

**Q9**
For `filter.msi`
```Zed
_path=="files" 185.10.68.235
```
For `10opd3r_load.msi`
```Zed
_path=="http" 192.36.27.92 | cut uri
```
2 lines for 2 files

**Q10**
Filter `filter.msi`
Here I needed wireshark -> Filter on Packet details | String, right click on the packet and Follow TCP Stream. The responses are hidden in the botton

**Q11**
Filter `10opd3r_load.msi`, same as Q10


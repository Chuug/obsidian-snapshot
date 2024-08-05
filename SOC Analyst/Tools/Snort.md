
https://tryhackme.com/r/room/snort

[CheatSheet](SnortCheatsheet-TryHackMe-1647343678716.pdf)

---

### Basics

**Check configuration file**
```shell
sudo snort -c /etc/snort/snort.conf -T
```

**Start in background**
```shell
sudo snort -c /etc/snort/snort.conf -D
```

**Find the process and kill it**
```shell
ps -ef | grep snort
sudo kill -9 [pid]
```

**Alert mode**
- **console**: Provides fast style alerts on the console screen.
- **cmg**: Provides basic header details with payload in hex and text format.
- **full:** Full alert mode, providing all possible information about the alert.  
- **fast:** Fast mode, shows the alert message, timestamp, source and destination ıp along with port numbers.
- **none:** Disabling alerting.

```shell
sudo snort -c /etc/snort/snort.conf -A console
```
### Sniffing

**Capture in verbose mode (`-v`) on selected interface (`-i`)**
```shell
sudo snort -v -i eth0
```

**Capture link layer (`-e`) and dump (`-d`)**
```shell
sudo snort -de
```

**Capture full packet mode (`-X`)**
```shell
sudo snort -X
```

### Logging
We can user other tools like wireshark or tcpdump to read the logs

**Logging a pcap**
<small>This will create a snort log file</small>
```shell
snort -r ftp-png-gif.pcap -l .
```

**Read the 10 first paquet on tcp 21 in the snort log file**
```shell
snort -r [snort-log-file] -d 'port 21' -n 10
```

**Read a pcap with rule**
```shell
snort -c local.rules -r [.pcap] -l . #will output a snortlog file
```

**Dump a pcap**
```shell
snort -r snort.log -d > dump.txt
```

### IDS/IPS Mode
**IDS**, for Intrusion **Detection** System is for generating alert on rules
**IPS**, for Intrusion **Prevention** System is for blocking traffic on rules

**Example of detection rule**
<small>This will generate an alert on traffic on port 80</small>
```text
alert tcp any any <> any 80 (msg:"Port 80";sid:1)
```

**Use the detection rule**
<small>Will generate logs on the current directory with "-l .", otherwise in /var/log/snort</small>
```shell
sudo snort -c rule -l .
```

**Example of prevention rule**
<small>This will block every ingoing and outgoing traffic to a port 80</small>
```text
drop tcp any any <> any 80 (msg:"Drop all port 80";sid:1)
```

**Use the prevention rule**
<small>Need more knowledge about this one</small>
```shell
sudo snort -c rule2 -q -Q --daq afpacket -i eth0:eth1 -A full -D -l .
```
### One ring to Rules them all

```shell
snort -c local.rules -r [.pcap] -l .
```

**Write a rule to filter IP `ID "35369" and run it against the given pcap file**
```text
alert icmp any any <> any any (msg:”ID Test”;id:35369;sid:1)
```

**Write a rule to filter packets with `Push-Ack` flags and run it against the given pcap file. What is the number of detected packets?**
```text
alert tcp any any <> any any (msg:”Test”;flags:PA;sid:1)
```

#wireshark 

https://tryhackme.com/r/room/wiresharktrafficanalysis

---

### Nmap Scans
**Q1. What is the total number of the "TCP Connect" scans?**
```Wireshark
tcp.flags.syn==1 && tcp.flags.ack==0 && tcp.wndow_size > 1024
```
Get all the first handshakes steps with the syn flag and a size > 1024

**Q3. How many "UDP close port" messages are there?**
```Wireshark
icmp.type==3 && icmp.code==3
```

**Q4. Which UDP port in the 55-70 port range is open?**
```Wireshark
udp.dstport in {55 .. 70}
```
Just have to see which one is not replying with an ICMP packet
### ARP Poisoning & Man In The Middle!
**Opcode 1: ARP requests**
```Wireshark
arp.opcode == 1
```

**Opcode 2: ARP responses**
```Wireshark
arp.opcode == 2
```

**Hunting ARP scanning**
```Wireshark
arp.dst.hw_mac == 00:00:00:00:00:00 
```

**Hunting ARP possible poisoning detection**
```Wireshark
arp.duplicate-address-detected or arp.duplicate-address-frame
```

**Hunting ARP possible flooding detection**
```Wireshark
((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address)
```

##### Questions

**Q1. What is the number of ARP requests crafted by the attacker?**
```Wireshark
((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == 00:0c:29:e2:18:b4)
```

**Q2. What is the number of HTTP packets received by the attacker?**
```Wireshark
eth.dst==00:0c:29:e2:18:b4 and http
```

**Q3. What is the number of sniffed username&password entries?**
I had to use the second filter and count with `&pass`
```Wireshark
eth.dst==00:0c:29:e2:18:b4 && http.request.method == "POST" && frame contains "&pass"
```

**Q4. What is the password of the "Client986"?**
```Wireshark
http && frame contains "client986"
```

**Q5. What is the comment provided by the "Client354"?**
```Wireshark
http && frame contains "client354"
```

### Identifying Hosts: DHCP, NetBIOS and Kerberos

##### DHCP Analysis
Global search are `dhcp` or `bootp`

**DHCP Requests packets that contains the hostname information**
```Wireshark
dhcp.option.dhcp == 3
```

**DHCP ACK accepted requests**
```Wireshark
dhcp.option.dhcp == 5
```

**DHCP NAK denied requests**
```Wireshark
dhcp.option.dhcp == 6
```

**Target hostname**
```Wireshark
dhcp.option.hostname contains "keyword"
```

**Target domain name**
```Wireshark
dhcp.option.domain_name contains "keyword"
```

##### NetBIOS (NBNS) Analysis
Global search is `nbns`

**Target name**
```Wireshark
nbns.name contains "keyword"
```

##### Kerberos Analysis
Global search is `kerberos`

**Target user account**
```Wireshark
kerberos.CNameString contains "keyword$"
```
Hostnames end with `$`, user account without `$`

**Target protocol version**
```Wireshark
kerberos.pvno == 5
```

**Target domain name**
```Wireshark
kerberos.realm.contains ".org"
```

**Target services**
```Wireshark
kerberos.SNameString == "krbtg"
```

##### Questions

**Q1. What is the MAC address of the host "Galaxy A30"?**
```Wireshark
dhcp.option.hostname contains "Galaxy" && dhcp.option.hostname contains "A30"
```
The contains `Galaxy` is enough to figure out

**Q2. How many NetBIOS registration requests does the "LIVALJM" workstation have?**
```Wireshark
nbns.flags.opcode == 5 && nbns.name contains "LIVAL"
```

**Q3.Which host requested the IP address "172.16.13.85"?**
```Wireshark
dhcp.option.requested_ip_address==172.16.13.85
```

**Q4. What is the IP address of the user "u5"?**
```Wireshark
kerberos.CNameString contains "u5"
```

**Q5. What is the hostname of the available host in the Kerberos packets?**
```Wireshark
kerberos.CNameString contains "$"
```

### Tunneling Traffic: DNS and ICMP
##### ICMP Analysis
Global search is `icmp`

**Target icmp custom packet (> 64 bytes data)**
```Wireshark
data.len > 64 && icmp
```

##### DNS Analysis
Global search is `dns`

**Target known patterns like `dnscat` and `dns2tcp`**
```Wireshark
dns contains "dnscat"
```

**Target long dns addresses with encoded subdomain addresses**
```Wireshark
dns.qry.name.len > 15 and !mdns
```
`!mdns` is for disabling local link device queries

##### Questions
**Q1. Investigate the anomalous packets. Which protocol is used in ICMP tunnelling?**
```Wireshark
data.len > 64 && icmp
```
We can see the SSH protocol by reading the hex data of packets

**Q2. Investigate the anomalous packets. What is the suspicious main domain address that receives anomalous DNS queries?**
```Wireshark
dns.qry.name.len > 100 && !mdns
```
Shows up very long encoded subdomain addresses with the domain `dataexfil.com`

### Cleartext Protocol Analysis: FTP
Global search is `ftp`

**Target response code**
```Wireshark
ftp.response.code == 211
```
[Response codes list](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes)
**230** : User login
**231** : User logout
**331** : Valid username
**430** : Invalid username or password
**530** : No login, invalid password (bruteforce)

**Target commands**
```Wireshark
ftp.request.command == "USER"
```
Commands: USER, PASS, CWD, LIST

**Target argument**
```Wireshark
ftp.request.arg == "password"
```

**Target bruteforce**
```Wireshark
ftp.response.code == 530
```

```Wireshark
(ftp.response.code == 530) && (ftp.response.arg contains "username")
```

```Wireshark
(ftp.request.command == "PASS") && (ftp.request.arg == "password")
```

**Q1. How many incorrect login attempts are there?**
```Wireshark
ftp.response.code == 530
```

**Q2. What is the size of the file accessed by the "ftp" account?**
```Wireshark
ftp.request.command == "SIZE"
```
Follow the TCP stream

**Q3. The adversary uploaded a document to the FTP server. What is the filename?**
The name of the file is in the TCP stream from above

**Q4. The adversary tried to assign special flags to change the executing permissions of the uploaded file. What is the command used by the adversary?**
Still in the TCP stream from above

### Cleartext Protocol Analysis: HTTP
##### HTTP Analysis
Global search is `http` or `http2` 

**Target request method**
```Wireshar
http.request.method == "GET"
```

**Target response status codes**
```Wireshark
http.response.code == 200
```
[QuickRef](https://quickref.me/http-status-code)

**Target user agent**
```Wireshark
http.user_agent contains "nmap"
```

**Target uri**
```Wireshark
http.request.uri contains "admin"
```

**Target full uri**
```Wireshark
http.request.full_uri contains "admin"
```

**Target server service name**
```Wireshark
http.server contains "apache"
```

**Target server hostname**
```Wireshark
http.host contains "keyword"
```

**Target connection status**
```Wireshark
http.connection == "Keep-Alive"
```

**Target web form information**
```Wireshark
data-text-lines contains "keyword"
```

##### User Agent Analysis
Global search is `http.user_agent`

**Big filter**
```Wireshark
(http.user_agent contains "sqlmap") or (http.user_agent contains "Nmap") or (http.user_agent contains "Wfuzz") or (http.user_agent contains "Nikto")
```

##### Log4j Analysis
The attack start with POST request
```Wireshark
http.request.method == "POST"
```

**Target patterns**
```Wireshark
(ip contains "jndi") || (ip contains "Exploit")
```

```Wireshark
(frame contains "jndi") || (frame contains "Exploit")
```

```Wireshark
(http.user_agent contains "$") || (http.user_agent contains "==")
```

##### Exercices
**Q1. Investigate the user agents. What is the number of anomalous  "user-agent" types?**
```Wireshark
http.user_agent
```
And you count anomalies like a retard

**Q2. What is the packet number with a subtle spelling difference in the user agent field?**
`Mozlila` replaced `Mozilla` 

**Q3. Locate the "Log4j" attack starting phase. What is the packet number?**
```Wireshark
http.request.method == "POST" && ip contains "jndi"
```

**Q4. Locate the "Log4j" attack starting phase and decode the base64 command. What is the IP address contacted by the adversary? (Enter the address in defanged format and exclude "{}".)**
Same filter as above, there is a base64 encoded string as payload with the ip in

### Encrypted Protocol Analysis: Decrypting HTTPS
Global search is `http.request` or `tls`

**Target handshake type**
```Wireshark
tls.handshake.type == 1 OR 2
```

**Target ssdp**
```Wireshark
ssdp
```
SSDP is a network protocol that provides advertisement and discovery of network services

**Target Client Hello**
```Wireshark
(http.request || tls.handshake.type == 1) && !(ssdp)
```

**Target Server Hello**
```Wireshark
(http.request || tls.handshake.type == 2) && !(ssdp)
```

**Add key log files**
<small>To do this, you will need to set up an environment variable and create the SSLKEYLOGFILE, and the browser will dump the keys to this file as you browse the web. SSL/TLS key pairs are created per session at the connection time, so it is important to dump the keys during the traffic capture.</small>
**Edit -> Preferences -> Protocols -> TLS -> (Pre)-Master-Secret log filename**

##### Exercices
**Q1. What is the frame number of the "Client Hello" message sent to "accounts.google.com"?**
```Wireshark
(http.request || tls.handshake.type == 1) && !(ssdp) && frame contains "accounts.google.com"
```
The correct way would be to do this long request above but the short way works
```Wireshark
frame contains "accounts.google.com"
```

**Q2. Decrypt the traffic with the "KeysLogFile.txt" file. What is the number of HTTP2 packets?**
```Wireshark
http2
```

**Q3. Go to Frame 322. What is the authority header of the HTTP2 packet?**
The `Authority header` is in the packet details

**Q4. Investigate the decrypted packets and find the flag! What is the flag?**
I had to use the subfilter on `flag.txt`and follow the http stream on the `HTTP` packet

### Bonus: Hunt Cleartext Credentials!

**Q1. What is the packet number of the credentials using "HTTP Basic Auth"?**
```Wireshark
frame contains "Auth"
```

**Q2. What is the packet number where "empty password" was submitted?**
```Wireshark
ftp.request.command == "PASS
```

### Bonus: Actionable Results!
**Q1 & Q2**
Just go to **Tools ->  Firewall ACL Rules -> Create rules for IPFirewall

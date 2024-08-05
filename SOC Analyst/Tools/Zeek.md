https://tryhackme.com/r/room/zeekbro

---

### Zeek service
**Start Zeek service**
```shell
sudo su
zeekctl start
```

**Zeek status**
```shell
zeekctl status
```

**Zeek stop**
```shell
zeekctl stop
```

**ZeekControl**
```shell
zeekctl
```


### Zeek commands
**Read pcap**
```shell
zeek -C -r [.pcap]
```



### Read logs
**Zeek-cut**
<small>Easily filter by fields</small>
```shell
cat conn.log | zeek-cut uid proto id.orig_h
```


### Signatures

##### Example 1
<center>http-password.sig</center>
```text
signature http-password { 
	ip-proto == tcp 
	dst-port == 80 
	payload /.*password.*/ 
	event "Cleartext Password Found!" 
}
```

```shell
zeek -C -r http.pcap -s http-password.sig
```

This will trigger an event each time a tcp paquet to the port 80 has the `password` payload in the packet

##### Example 2
<center>ftp-brute-force.sig</center>
```text
signature ftp-username { 
	ip-proto == tcp 
	ftp /.*USER.*/ 
	event "FTP Username Input Found!" 
} 

signature ftp-brute { 
	ip-proto == tcp 
	payload /.*530.*Login.*incorrect.*/ 
	event "FTP Brute-force Attempt!" 
}
```

```shell
zeek -C -r ftp.pcap -s ftp-brute-force.sig
```

This will trigger each time a user tries to login and each time someone fail at login and may be try to bruteforce


### Zeek Scripts <small>.zeek</small>
**Scripts location**
```text
/opt/zeek/share/zeek/site
```


##### Example 1

<center>The sample</center>
```text
ubuntu@ubuntu$ zeek -C -r sample.pcap 102.zeek 
[
	id=[orig_h=192.168.121.40, orig_p=123/udp, resp_h=212.227.54.68, resp_p=123/udp], 
	orig=[size=48, state=1, num_pkts=0, num_bytes_ip=0, flow_label=0, l2_addr=00:16:47:df:e7:c1], 
	resp=[size=0, state=0, num_pkts=0, num_bytes_ip=0, flow_label=0, l2_addr=00:00:0c:9f:f0:79], 
	start_time=1488571365.706238, 
	duration=0 secs, 
	service={}, 
	history=D, 
	uid=CajwDY2vSUtLkztAc, 
	tunnel=, 
	vlan=121, inner_vlan=, dpd=, dpd_state=, removal_hooks=, conn=, extract_orig=F, extract_resp=F, thresholds=, dce_rpc=, dce_rpc_state=, dce_rpc_backing=, dhcp=, dnp3=, dns=, dns_state=, ftp=, ftp_data_reuse=F, ssl=, http=, http_state=, irc=, krb=, modbus=, mysql=, ntlm=, ntp=, radius=, rdp=, rfb=, sip=, sip_state=, snmp=, smb_state=, smtp=, smtp_state=, socks=, ssh=, syslog=]
```

<center>The script</center>
```text
event new_connection(c: connection) 
{ 
	print ("###################################################"); 
	print (""); 
	print ("New Connection Found!"); 
	print (""); 
	print fmt ("Source Host: %s # %s --->", c$id$orig_h, c$id$orig_p); 
	print fmt ("Destination Host: resp: %s # %s <---", c$id$resp_h, c$id$resp_p); 
	print (""); 
} 
# %s: Identifies string output for the source. 
# c$id: Source reference field for the identifier.
```

<center>The result</center>
```text
ubuntu@ubuntu$ zeek -C -r sample.pcap 103.zeek ########################################################### 
New Connection Found! Source Host: 192.168.121.2 # 58304/udp ---> Destination Host: resp: 192.168.120.22 # 53/udp <---
```

##### Example 2 - Combine with Signatures

<center>The script - 201.zeek</center>
```text
event signature_match (state: signature_state, msg: string, data: string) 
{ 
	if (state$sig_id == "ftp-admin") 
		{ print ("Signature hit! --> #FTP-Admin "); 
	} 
}
```

<center>The signature - ftp-admin.sig</center>
```text
signature ftp-admin { 
	ip-proto == tcp 
	ftp /.*USER.*admin.*/ 
	event "FTP Username Input Found!" 
}
```

```shell
zeek -C -r ftp.pcap -s ftp-admin.sig 201.zeek
```

<center>ouput</center>
```text
Signature hit! --> #FTP-Admin Signature hit! --> #FTP-Admin 
Signature hit! --> #FTP-Admin Signature hit! --> #FTP-Admin
...
...
Signature hit! --> #FTP-Admin Signature hit! --> #FTP-Admin
```

### Frameworks

##### Example 1
<center>hash-demo.zeek</center>
```zeek
@load /opt/zeek/share/zeek/policy/frameworks/files/hash-all-files.zeek
```

This call the `hash-all-files.zeek` framework

<center>hash-all-files.zeek</center>
```zeek
# Enable MD5, SHA1 and SHA256 hashing for all files. 

@load base/files/hash 
event file_new(f: fa_file) 
{ 
	Files::add_analyzer(f, Files::ANALYZER_MD5); 
	Files::add_analyzer(f, Files::ANALYZER_SHA1); 
	Files::add_analyzer(f, Files::ANALYZER_SHA256); 
}
```

The `hash-all-files.zeek` calls another framework `base/files/hash`

##### Example 2 - Extract Files
```shell
zeek -C -r case1.pcap /opt/zeek/share/zeek/policy/frameworks/files/extract-all-files.zeek
```
Will extract all the files from the pcap

### Package Manager
Root required

**Install a package**
```shell
zkg install [package-path-or-url]
```

**List packages**
```shell
zkg list
```

**Remove package**
```shell
zkg remove [package]
```

**Check update**
```shell
zkg refresh
```

**Do the update**
```shell
zkg upgrade
```




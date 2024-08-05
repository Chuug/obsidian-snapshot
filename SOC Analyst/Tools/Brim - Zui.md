[Deb package](https://sourceforge.net/projects/brim.mirror/)

**Count packet by `_path` and sort in reverse**
```Zed
count() by _path | sort -r
```

**Cut specific fields**
```Zed
_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h
```

**List unique values**
```Zed
_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h | sort | uniq
```

**Create variable**
```Zed
put var := "hello" | cut var
```
### Common Queries
**Communicated Hosts**
```Zed
_path=="conn" | cut id.orig_h, id.resp_h | sort | uniq
```

**Frequently communicated Hosts**
```Zed
_path="conn" | cut id.orig_h, id.resp_h | sort | uniq -c | sort -r
```

**Most active ports**
```Zed
_path=="conn" | cut id.resp_p, service | sort | uniq
```

```Zed
_path=="conn" | cut id.orig_h, id.resp_h, id.resp_p, service | sort id.resp_p | uniq -c | sort -r
```

**Long Connections**
```Zed
_path=="conn" | cut id.orig_h, id.resp_h, duration | sort -r duration
```

**Transferred Data**
```Zed
_path=="conn" | put total_bytes := orig_bytes + resp_bytes | sort -r total_bytes | cut uid, id, orig_bytes, resp_bytes, total_bytes
```

**DNS and HTTP queries**
```Zed
_path=="dns" | count() by query | sort -r
```

```Zed
_path=="http" | count() by uri | sort -r
```

**Suspicious hostnames**
```Zed
_path=="dhcp" | cut host_name, domain
```

**Suspicious IP Addresses**
```Zed
_path=="conn" | put classnet := network_of(id.resp_h) | cut classnet | count() by classnet | sort -r
```

**Detect files**
```Zed
filename!=null
```

**SMB activity**
```Zed
_path=="dce_rpc" OR _path=="smb_mapping" OR _path=="smb_files"
```

**Known patterns**
```Zed
event_type=="alert" OR _path=="notice" OR _path=="signatures"
```

##### Queries from the old Brim
**Windows Networking Activity**
```Zed
_path matches smb* OR _path=="dce_rpc"
```

**Unique DNS Queries**
```Zed
_path=="dns" | count() by query | sort -r
```

**HTTP Requests**
```Zed
_path=="http" | cut id.orig_h, id.resp_h, id.resp_p, method, host, uri | uniq -c
```

**Unique Network Connections**
```Zed
_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h | sort | uniq
```

**Connection Received Data**
```Zed
_path=="conn" | put total_bytes := orig_bytes + resp_bytes | sort -r total_bytes | cut uid, id, orig_bytes, resp_bytes, total_bytes
```

**File Activity**
```Zed
filename!=null | cut _path, tx_hosts, rx_hosts, conn_uids, mime_type, filename, md5, sha1
```

**HTTP Post Requests**
```Zed
method=="POST" | cut ts, uid, id, method, uri, status_code
```

**Show IP Subnets**
```Zed
_path=="conn" | put classnet := network_of(id.resp_h) | cut classnet | count() by classnet | sort -r
```

**Suricata Alerts by Category**
```Zed
event_type=="alert" | count() by alert.severity,alert.category | sort count
```

**Suricata Alert by Source and Destination**
```Zed
event_type=="alert" | alerts := union(alert.category) by src_ip, dest_ip
```

**Suricata Alert by Subnet**
```Zed
event_type=="alert" | alerts := union(alert.category) by network_of(dest_ip)
```





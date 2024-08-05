### Dump using FTK Image
In Forensic room



### Volatility
<small>I put it in ~/Applications</small>

**Read the fxcking manual**
```shell
vol.py -h
```

#### Read dump
Choose betwenn **windows.[param]** | **linux.[param]** | **mac.[param]**

**Determine dump info and OS**
```shell
vol.py -f [dump] windows.info
```


**Basic processes listing**
```shell
vol.py -f [dump] windows.pslist
```

**Advanced processes listing**
<small>Can see evaded malicious processes but can cause false positive</small>
```shell
vol.py -f [dump] windows.psscan
```

**List tree processes**
<small>Kind of useless because we can see the PPID (Parent PID) with the above listing, it might be a little bit more visual and it doesn't show evaded processes</small>
```shell
vol.py -f [dump] windows.pstree
```

**List DDL's used by each processes**
```shell
vol.py -f [dump] windows.dlllist
```

**Show network**
```shell
vol.py -f [dump] windows.netstat
```

**Try to find injected processes and their PIDs**
```shell
vol.py -f [dump] windows.malfind
```

##### Advanced malware scan
**ssdt scan**
```shell
vol.py -f [dump] windows.ssdt
```

**Module scan**
```shell
vol.py -f [dump] windows.modules
```

**Driver scan**
```shell
vol.py -f [dump] windows.driverscan
```

##### PID dump
**Dump memory from a PID**
```shell
vol.py -f [dump] -o [output] --pid [pid] --dump
```

**Then search in the dump with grep**
```shell
strings [pid-dump] | grep -i [keyword]
```


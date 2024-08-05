### Read
**Read pcap infos**
```shell
capinfos [.pcap]
```

**Write (`-w`) sniffing in pcap silently (`-q`)**
```shell
sudo tshark -w output.pcap -q
```

**Read (`-r`) pcap**
<small>Read the 10 first packets</small>
```shell
sudo tshark -r [.pcap] -c 10
```

**Read first (`-c 1`) packet bytes (`-x`) and packets details (`-V`)**
```shell
sudo tshark -r [.pcap] -c 1 -x -V
```

**Filter (`-Y`) the 5th packet**
```shell
sudo tshark -r [.pcap] -Y 'frame.number == 5'
```

**Filter (`-Y`) by IP and count ouput (`nl`)**
```shell
sudo tshark -r [.pcap] -Y 'ip.addr == 192.168.1.1' | nl
```

### Capture
**List interfaces (`-D`)**
```shell
sudo tshark -D
```

**Capture on first interface**
<small>Usually eth0</small>
```shell
sudo tshark
```

**Capture on selected interface (`-i`)**
```shell
sudo tshark -i 2 #number from the list
# OR
sudo tshark -i tun0 #interface name
```

**Filter (`-f`) on tcp**
```shell
sudo tshark -f "tcp"
```

**Filter (`-f`) source host**
```shell
sudo tshark -f "src host 10.10.10.10"
```

**Filter (`-f`) destination host**
```shell
sudo tshark -f "dst host 10.10.10.10"
```

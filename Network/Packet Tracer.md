 ### DHCP Server setup

#### On server
##### Switch:
`en`
`conf t`
`int vlan1`
`ip address [ip-switch] [mask]`
`no sh`
`exit`
`ip default-gateway [gateway]
<small>The [ip-switch] should be the gateway + 1 (ex 192.168.1.2)</small>
##### Router:
`int [interface]`
`ip helper-address [ip-dhcp-server]

#### On router
`enable`
`conf t`
`int [interface]`
`ip address [ip] [mask]`
`no shut`
`do write memory`
`ip dhcp pool [name]`
`network [ip] [mask]`
`exit`



### Firewall setup
##### Inside interface configuration
`int [interface]`
`nameif inside security-level 100`
`ip address [gateway] [mask]`
`no sh`
##### Outside interface configuration
`int [interface]
`nameif outside security-level 0`
`ip address [gateway] [mask]` 
`no sh`
##### Authorize ICMP traffic from inside to outside
`access-list IN-OUT extended permit icmp any any`
##### Authorize ICMP replies only from outside to inside
`access-list OUT-IN extended permit icmp any any echo-reply `
`access-group OUT-IN in interface outside` 
##### Apply ACL to inside interface
`access-group IN-OUT in interface inside`
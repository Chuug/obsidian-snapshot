
https://tryhackme.com/r/room/tsharkchallengestwo

---

```shell
tshark -r directory-curiosity.pcap -Y "http.request" -T fields -e http.host
```
This gave me the domain

```shell
tshark -r directory-curiosity.pcap -Y "http.request" -T fields -e http.host -e ip.dst | sort | uniq -c | sort -r
```
This gave me the number of request sent and the ip
